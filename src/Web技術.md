# Web技術

HTML、CSS、JavaScript等、広くWeb(World Wide Web, WWW)に関する技術について扱う。

- [Web 関連仕様 日本語訳](https://triple-underscore.github.io/index.html)
    - Web関連の諸々の規格書の和訳を公開してくれている非常にありがたいサイト
    - ものによっては他にも和訳が存在するものがあるが、このサイトの訳では語彙をカタカナ語のままにせず漢語に訳している傾向が強いので個人的に好き。

## HTML
Hypertext Markup Language(HTML)は、Webページの構造を記述するための言語。

長らくWorld Wide Web Consortium(W3C)が規格を決めてきたが、Web Hypertext Application Technology Working Group(WHATWG)という対抗組織が結成されて、W3C系とは別のHTML規格を作るなどしていた。
結局WHATWG側が勝利(?)し、W3C側のHTMLはHTML5.2で打ち止めになり、以降はWHATWG側のバージョンのつかない "HTML Living Standard" のほうに一本化されることになった。

- 詳しい歴史の話は [HyperText Markup Language - Wikipedia](https://ja.wikipedia.org/wiki/HyperText_Markup_Language) などを参照。
- [HTML Living Standard](https://html.spec.whatwg.org/multipage/): 2020年現在のHTML規格
    - これが随時更新されていく
    - [和訳（@triple-underscore氏による）](https://triple-underscore.github.io/index.html#spec-list-html)
    - [和訳（@momdo氏による）](https://momdo.github.io/html/)
    - [開発者版（簡易版）の和訳（@momdo氏による）](https://momdo.github.io/html/dev/)

### ルビに関して
前述の通り、W3CとWHATWGで微妙に異なった規格が作られていた後WHATWG側に統合された経緯から、W3C側のHTML5にのみ存在した機能の一部が破棄されている。

- 主な差異については[HTML Living Standard - とほほのWWW入門](http://www.tohoho-web.com/html/memo/htmlls.htm)の「HTML 5.2 と HTML Living Standard の差分」の節を参照。
- HTML5系との差異に限らず、現在のHTML Living Standardで廃止された機能については[HTML Standard — Obsolete features（日本語訳）](https://triple-underscore.github.io/HTML-obsolete-ja.html)を参照。

中でも日本人が引っかかりがちなのがルビ（振り仮名）に関する部分である。

HTML5では、

```html
<ruby>
<rb>朝</rb><rb>寝</rb><rb>坊</rb>
<rtc><rt>あさ</rt><rt>ね</rt><rt>ぼう</rt></rtc>
</ruby>
```
出典: [HTML5/テキスト/ruby要素 ルビ（ふりがな）を振る - TAG index](https://www.tagindex.com/html5/text/ruby.html)

のように`rb`タグと`rtc`タグで熟語ルビをマークアップすることができたが、`rb`、`rtc`タグはWHATWG版では廃止された機能となった。

現行の規格の[ルビに関する説明](https://triple-underscore.github.io/HTML-text-level-ja.html#the-ruby-element)では熟語ルビについて触れられているが、

```html
<ruby>
朝<rt>あさ</rt>寝<rt>ね</rt>坊<rt>ぼう</rt>
</ruby>
```
のように、HTMLの層ではモノルビと同じマークアップを行い、CSS等の層で区別することになったようである。

- 要するにモノルビと熟語ルビは意味論的な差異ではなく表示上の差異であるということに変更されたということであろうが、些か疑問である。
- `rb`、`rtc`タグは廃止されたものの、HTML5時代に既に実装していた[Firefoxでは正しく表示できる](https://developer.mozilla.org/ja/docs/Web/HTML/Element/rtc)。
    - Firefoxでだけ確認していると何故か他のブラウザで表示がバグるということになるので注意[^1]。もちろん、Firefoxで動くからといって以降この機能を使うべきではない。
- 「HTML」で検索するとHTML4時代のサイトが、「HTML5」で検索するとW3CのHTML5についてのサイトが引っかかる以上、最新の「HTML」の情報を検索するにはどうすればいいんだろうか……。

[^1]: 私がそれに嵌まった。

## JavaScript

- [ECMAScript® 2021 Language Specification](https://tc39.es/ecma262/)
    - ”JavaScript” にはブラウザごとに諸々の実装があるが、その基準となるのがECMAScriptである。
    - [Standard ECMA-262](https://www.ecma-international.org/publications/standards/Ecma-262.htm)が1年毎に更新されるが、こちらはスナップショットに過ぎず、上記リンクの随時更新されるLiving Standardが主であるらしい（参考：[ECMAScript · JavaScript Primer #jsprimer](https://jsprimer.net/basic/ecmascript/)）。
- [JavaScript Primer - 迷わないための入門書 #jsprimer](https://jsprimer.net/)
    - ECMAScript 2015以降（≒現代的なJavaScript）に準拠した分かりやすい入門書

### DOM操作
- [セレクターを使用した DOM 要素の特定 - Web API | MDN](https://developer.mozilla.org/ja/docs/Web/API/Document_object_model/Locating_DOM_elements_using_selectors): DOM要素を選択する簡単な方法
- [DOM ツリーの作成方法 - Web API | MDN](https://developer.mozilla.org/ja/docs/Web/API/Document_object_model/How_to_create_a_DOM_tree): 新たに生成したDOM要素をページに追加する方法
- [イベントへの入門 - ウェブ開発を学ぶ | MDN](https://developer.mozilla.org/ja/docs/Learn/JavaScript/Building_blocks/Events): イベントの追加方法
- [HTMLElement - Web API | MDN](https://developer.mozilla.org/ja/docs/Web/API/HTMLElement)
    - DOM要素はHTMLElementを継承したクラスなので、どういうメンバ変数・メンバ関数を持っているか（例えば`innerText`など）は、基本的にはこのHTMLElementのドキュメントを見ると良い。なお、HTMLElementではなくその基底クラスであるElementで定義されているものもたくさんあるが、このページには出ていない（Elementのページに飛ぶ必要がある）ため注意。
    - HTMLElementにはない、あるタグだけが持っている特殊な性質の操作が必要な場合は、タグの種類ごとにどのようなクラスが返ってくるかを知る必要がある。これは、[HTML 要素リファレンス - HTML: HyperText Markup Language | MDN](https://developer.mozilla.org/ja/docs/Web/HTML/Element)のページから飛べるタグのページに「DOMインターフェース」という項目で示されている。
    - 例えば[`<canvas>`](https://developer.mozilla.org/ja/docs/Web/HTML/Element/canvas)の場合、[HTMLCanvasElement](https://developer.mozilla.org/ja/docs/Web/API/HTMLCanvasElement)要素で返ってくる。このHTMLCanvasElementのメンバを使えば、canvasに特有の幅や高さといった情報を取得・設定することができるのである。

#### 例:さいころを振る

```html
<!DOCTYPE html>
<html lang="ja">

<body>
    <div id="app">
        <!-- ここにいろいろ要素を追加していく -->
    </div>
    <script>
        // DOM要素の取得
        const masterEl = document.querySelector("#app");
        // 追加したい要素の生成
        const btn = document.createElement("button");
        btn.innerText = "さいころを降る";
        // 要素を子として追加
        masterEl.appendChild(btn);
        // イベントの登録
        btn.addEventListener("click", (e) => {
            // 1から6のランダムな整数
            const rnd = Math.floor(Math.random() * 6 + 1);
            // 要素を生成
            const p = document.createElement("p");
            p.innerText = rnd;
            // 要素を追加
            // なお、JavaScriptの函数は函数閉包がつくられるため、masterEl変数が使える
            masterEl.appendChild(p);
        });
    </script>
</body>

</html>
```

<div id="app">
    <!-- ここにいろいろ要素を追加していく -->
</div>
<script>
    // DOM要素の取得
    const masterEl = document.querySelector("#app");
    // 追加したい要素の生成
    const btn = document.createElement("button");
    btn.innerText = "さいころを降る";
    // 要素を子として追加
    masterEl.appendChild(btn);
    // イベントの登録
    btn.addEventListener("click", (e) => {
        // 1から6のランダムな整数
        const rnd = Math.floor(Math.random() * 6 + 1);
        // 要素を生成
        const p = document.createElement("p");
        p.innerText = rnd;
        // 要素を追加
        // なお、JavaScriptの函数は函数閉包がつくられるため、masterEl変数が使える
        masterEl.appendChild(p);
    });
</script>

## TypeScript
- [TypeScript: Typed JavaScript at Any Scale.](https://www.typescriptlang.org/ja/#): 公式サイト
    - [公式ドキュメント目次](https://www.typescriptlang.org/docs)
    - [The TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html): 文法便覧
- [TypeScript入門以前ガイド - mizchi's blog](https://mizchi.hatenablog.com/archive/2018/10/03)
- uhyo氏の型まわりの解説記事群
    - [TypeScriptの型入門 - Qiita](https://qiita.com/uhyo/items/e2fdef2d3236b9bfe74a)
    - [TypeScriptの型初級 - Qiita](https://qiita.com/uhyo/items/da21e2b3c10c8a03952f)
    - [TypeScriptの型演習 - Qiita](https://qiita.com/uhyo/items/e4f54ef3b87afdd65546)
- [仕事ですぐに使えるTypeScript](https://future-architect.github.io/typescript-guide/)
- [TypeScript + Node.js プロジェクトのはじめかた2020 - Qiita](https://qiita.com/notakaos/items/3bbd2293e2ff286d9f49)
- [コンパイル結果で考えるTypeScriptのimport / export / namespace - stone's throw](https://osamtimizer.hatenablog.com/entry/2018/06/27/222155)

## Node.js
サーバーサイドJavaScript実行環境。
サーバーサイドということは他のよくある軽量プログラム言語と同じ立場なので、Web技術かというと怪しいが……

- [Node.js](https://nodejs.org/ja/): 公式サイト
    - [ガイド | Node.js](https://nodejs.org/ja/docs/guides/): チュートリアル
    - [Node.js v14.10.1 Documentation](https://nodejs.org/api/index.html) 最新版APIリファレンス
