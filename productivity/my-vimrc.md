# My vimrc

```text
filetype plugin indent on
" show existing tab with 4 spaces width
set tabstop=2
" when indenting with '>', use 4 spaces width
set shiftwidth=2
" On pressing tab, insert 4 spaces
set expandtab

nmap =j :%!python -m json.tool

" toggle auto-indentation"
nnoremap <F2> :set invpaste paste?<CR>
set pastetoggle=<F2>
set showmode

" map page up and page down keyboard keys to scoll up and down 
" without messing the curson column position
nnoremap <PageUp> <C-U>
nnoremap <PageDown> <C-D>
inoremap <PageUp> <C-O><C-U>
inoremap <PageDown> <C-O><C-D>
set nostartofline

set runtimepath^=/home/kp/.vim/pack/plugins/tagbar

"runtimepath=~/.vim,~/.vim/pack/plugins/start/vimwiki,/var/lib/vim/addons,/usr/share/vim/vimfiles,/usr/share/vim/vim80,/usr/share/vim/vimfiles/after,/var/lib/vim/addons/after,~/.vim/after
" gotags tagbar config
let g:tagbar_type_go = {
  \ 'ctagstype' : 'go',
  \ 'kinds'     : [
    \ 'p:package',
    \ 'i:imports:1',
    \ 'c:constants',
    \ 'v:variables',
    \ 't:types',
    \ 'n:interfaces',
    \ 'w:fields',
    \ 'e:embedded',
    \ 'm:methods',
    \ 'r:constructor',
    \ 'f:functions'
  \ ],
  \ 'sro' : '.',
  \ 'kind2scope' : {
    \ 't' : 'ctype',
    \ 'n' : 'ntype'
  \ },
  \ 'scope2kind' : {
    \ 'ctype' : 't',
    \ 'ntype' : 'n'
  \ },
  \ 'ctagsbin'  : '/home/kp/.go/bin/gotags',
  \ 'ctagsargs' : '-sort -silent'
\ }
nmap <F8> :TagbarToggle<CR>

```

