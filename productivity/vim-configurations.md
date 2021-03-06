# Vim configurations, Mappings and Ex Commands

```text
Change buffer without saving changes
:set hidden
```

```text
# backspace button in insert mode
:set backspace=start,eol,indent
```

```text
:filetype plugin indent on
```

```text
:syntax on
```

```text
# source a file
:so 
```

```text
# list all arguments
:args
```

```text
#jump to the first file in arguments
:first
```

```text
# set indentation level to be 4
:set shiftwidth=4
```

```text
# set 4 spaces to tab
:set tabstop=4
```

```text
# show tabs, eol characters
:set list
# hide
:set list!
```

```text
# show command history
:his
# OR
q:

# show search history
:his /
# OR
q/

# to open history when either in command or search mode
:s #start typing a command and hit ctrl F
/se # start typing a search and hit ctrl F
```

```text
# highlight and jump during search
:set incsearch
:set noincsearch

# highlight all matching search terms
:set hls
:set nohls
```

```text
# list the current path variable
:set path?

# set path to all files under src
:set path=.,app/src/**
:set wildignore=*.pyc

# find command doesn't need the absolute path of the file
:find root.js

# list vim runtime path
:set rtp?
```

```text
# turn out swap files
:set noswapfile
```

```text
set configurations which apply to the current buffer
:setlocal

TODO:
Find out how this applies when applied from a vimrc file.
```

```text
# open a terminal inside VIM
:term
:vert term
```

```text
# copy
:<range>co <address>
:<range>t <address>

# copy line matching the search keyword to the current line
:?<searchkey>?t.
```

```text
# define a regex to match the imports for any language
:set inlcude=^\s*from\s*\w*import\s*
:set suffixesadd=.js

# jump the first match on the current keyword
:ij <keyword>
```

### Mappings

```text
# jump to a file under current cursor position
gf

# set suffixes if missing
:set suffixesadd+=.js
```

```text
# show 1st match of keyword under cursor
[i

# show all line that match the keyword under cursor
[I

# jump to the first line that matches the keyword under cursor
[ ctrl-I

# jump to the 2nd matched line for the keyword under cursor
2[ctrl-I

# keybowrd mapping for interactive jump to matched line
map <F4> [I:let nr = input("Which one: ")<Bar>exe "normal " . nr ."[\t"<CR>
```

