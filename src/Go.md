# Go

Google製静的型付けプログラミング言語。シンプルさが売り。

## Webアプリ
- [Writing Web Applications - The Go Programming Language](https://golang.org/doc/articles/wiki/)
- [Nginxでecho(golang)でHello world - 自分用めも](https://belhb.hateblo.jp/entry/2019/01/08/192010)

## vimの設定
Language Server Protocol系を入れる。
保存時の自動整形はvim-goimportsを使う。

- [Big Sky :: Vim をモダンな IDE に変える LSP の設定](https://mattn.kaoriya.net/software/vim/20200106103137.htm)
- [vim-goからvim-lspへ移行しました - Carpe Diem](https://christina04.hatenablog.com/entry/migrate-from-vim-go-to-vim-lsp)

```vim
# プラグインを入れる
call plug#begin('~/.vim/plugged')
Plug 'prabirshrestha/asyncomplete.vim'
Plug 'prabirshrestha/asyncomplete-lsp.vim'
Plug 'prabirshrestha/vim-lsp'
Plug 'mattn/vim-lsp-settings'
Plug 'mattn/vim-goimports'
call plug#end()

" goplsまわりの設定
inoremap <expr> <Tab>   pumvisible() ? "\<C-n>" : "\<Tab>"
inoremap <expr> <S-Tab> pumvisible() ? "\<C-p>" : "\<S-Tab>"
let g:lsp_diagnostics_enabled = 1
let g:lsp_diagnostics_echo_cursor = 1
let g:asyncomplete_auto_popup = 1
let g:lsp_text_edit_enabled = 0 
```
