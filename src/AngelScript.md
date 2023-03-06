# AngelScript
C/C++のプログラムに組み込むためのスクリプト言語。要するにLuaみたいなやつ。
親プログラムの実行時にスクリプトをコンパイルし、組み込んだ処理系で処理する（事前にコンパイルすることも可能）。

売りは、

- C++風の文法
  - C++を知っている人ならほぼ迷わない
- 静的型付け
  - 型エラーはスクリプトのコンパイル時にわかる
- オブジェクト指向
- オブジェクトハンドル（≒スマートポインタ）によるメモリ管理
- UTF-8対応

あたり。

## リンク
- [AngelCode.com](https://www.angelcode.com/): 公式


## Hello, world
[リポジトリ](https://github.com/suzusime/etude-angel)

この例では[[CMake]]を用いる。
AngelScriptのバージョンは2.34.0。

### ディレクトリ構成
```
etude-angel（プロジェクトルート）
├── CMakeLists.txt
├── etude-angel.cpp
├── extern
│   └── angelsdk
│       ├── add_on
│       ├── angelscript
│       ├── docs
│       └── samples
└── hello.as
```

ダウンロードしたAngelScriptのSDKを解凍すると出てくる`sdk`ディレクトリを`angelsdk`と改名の上`extern`以下に配置するものとする。

### etude-angel.cpp
```cpp
#include <iostream>
#include <cassert>

#include <angelscript.h>
#include <scriptstdstring/scriptstdstring.h>
#include <scriptbuilder/scriptbuilder.h>

// スクリプト中から呼ぶヘルパー函数
void print(std::string &str)
{
    std::cout << str;
}

// メッセージコールバック函数
// コンパイルエラー等のメッセージはここで出力される
void MessageCallback(const asSMessageInfo *msg, void *param)
{
	const char *type = "ERR ";
	if( msg->type == asMSGTYPE_WARNING ) 
		type = "WARN";
	else if( msg->type == asMSGTYPE_INFORMATION ) 
		type = "INFO";

	printf("%s (%d, %d) : %s : %s\n", msg->section, msg->row, msg->col, type, msg->message);
}

int RunApplication(){
    using namespace std;
    int r = 0; //エラーコード等の返値を入れる

    // --- 準備部 --- //
    // スクリプトエンジンの作成
	asIScriptEngine *engine = asCreateScriptEngine();
	if( engine == 0 )
	{
		cout << "スクリプトエンジンの作成に失敗" << endl;
		return -1;
	}
    
    // コンパイラのメッセージをメッセージコールバック関数に送るようにする
	engine->SetMessageCallback(asFUNCTION(MessageCallback), 0, asCALL_CDECL);

    // string アドオンの登録
    RegisterStdString(engine);

    // print函数をスクリプトエンジンにグローバル函数として登録
    r = engine->RegisterGlobalFunction("void print(const string &in)", asFUNCTION(print), asCALL_CDECL); assert(r>=0);

    // スクリプトビルダーの作成
    CScriptBuilder builder;

    // モジュールの登録
    builder.StartNewModule(engine, "Hello");

    // スクリプトファイルの読み込み
    if( builder.AddSectionFromFile("hello.as") < 0){
        printf("スクリプトファイルの読み込みに失敗");
    }
    
    // スクリプトのビルド
    builder.BuildModule();

    // --- 実行部 ---//
    // モジュールの取得
    asIScriptModule* mod = engine->GetModule("Hello");

    // 函数の取得
    asIScriptFunction* func = mod->GetFunctionByDecl("void main()");

    // 実行文脈の作成
    asIScriptContext *ctx = engine->CreateContext();
    
    //実行
    for(int i=0; i<5; ++i){
        ctx->Prepare(func);
        ctx->Execute();
    }

    // コンテキストの解放
    ctx->Release();

    // スクリプトエンジンの終了
	engine->ShutDownAndRelease();
}

// エントリポイント
int main(){
    RunApplication();
    return 0;
}
```

- メインのプログラム
- 公式チュートリアルをもとにエラー処理などを省略した。

### hello.as
```cpp
void main()
{
    print("Hello, world!\n");
}
```

- etude-angel.cppから読み込まれるスクリプト。

### CMakeLists.txt
```cmake
cmake_minimum_required(VERSION 3.14)

project(etude-angel)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(ANGELSCRIPT_LIBRARY_NAME angelscript) # OS X Framework以外と仮定

set(ANGEL_SDK_DIR ${CMAKE_CURRENT_SOURCE_DIR}/extern/angelsdk)
add_subdirectory(${ANGEL_SDK_DIR}/angelscript/projects/cmake)

set(ANGEL_ADDON_SRCS
    ${ANGEL_SDK_DIR}/add_on/scriptbuilder/scriptbuilder.cpp
    ${ANGEL_SDK_DIR}/add_on/scriptstdstring/scriptstdstring.cpp
    )

add_executable(etude-angel etude-angel.cpp ${ANGEL_ADDON_SRCS})
target_link_libraries(etude-angel ${ANGELSCRIPT_LIBRARY_NAME})
target_include_directories(etude-angel PRIVATE
    ${ANGEL_SDK_DIR}/angelscript/include
    ${ANGEL_SDK_DIR}/add_on
    )

# コンパイラ依存のオプション
if(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    # aliasがstrictな前提の最適化をしない
    target_compile_options(etude-angel PRIVATE "-fno-strict-aliasing")
elseif(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    # BOM無しUTF-8で書かれたソースコードを受け付け、入出力もUTF-8にする
    target_compile_options(etude-angel PRIVATE "/utf-8")
endif()

# スクリプトファイルを実行フォルダにコピー
add_custom_command(TARGET etude-angel POST_BUILD COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_SOURCE_DIR}/hello.as $<TARGET_FILE_DIR:etude-angel>)
```

- AngelScript SDKはCMake用のビルドファイルを含んでいるのでCMakeから簡単にリンクできる。
- アドオンを使う場合は該当するソースコードをコンパイル対象に含める必要があることに注意。

### ビルド・実行
#### GCC + GnuMake
```sh
$ cmake -B build && (cd build && make && ./etude-angel)
```

#### Visual Studio 2019
ディレクトリをVisual Studioで開き、`etude-angel.exe`を選んでデバッグ実行すればよい。
