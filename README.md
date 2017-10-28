# vim
my vim config
# vimrc

"Must be first line
"禁用vi兼容模式
set nocompatible

" 设置背景色，每种配色有两种方案，一个light、一个dark
set background=dark
set shell=/bin/bash
set t_Co=256

"很多插件都会要求的配置检测文件类型
" 开启文件类型侦测
" 根据侦测到的不同类型加载对应的插件
" 自动缩进
filetype plugin indent on

"开启语法高亮
syntax on

set helplang=cn

set timeoutlen=500
set clipboard+=unnamed
set lazyredraw
set scrolloff=8
set visualbell

scriptencoding utf-8
set encoding=utf-8

"有时中文会显示乱码，用一下几条命令解决
let &termencoding=&encoding
set fileencodings=utf-8,gbk

    " Windows Compatible {
        " On Windows, also use '.vim' instead of 'vimfiles'; this makes synchronization
        " across (heterogeneous) systems easier.
        if has('win32') || has('win64')
          set runtimepath=$HOME/.vim,$VIM/vimfiles,$VIMRUNTIME,$VIM/vimfiles/after,$HOME/.vim/after

          " Be nice and check for multi_byte even if the config requires
          " multi_byte support most of the time
          if has("multi_byte")
            " Windows cmd.exe still uses cp850. If Windows ever moved to
            " Powershell as the primary terminal, this would be utf-8
            set termencoding=cp850
            " Let Vim use utf-8 internally, because many scripts require this
            set encoding=utf-8
            setglobal fileencoding=utf-8
            " Windows has traditionally used cp1252, so it's probably wise to
            " fallback into cp1252 instead of eg. iso-8859-15.
            " Newer Windows files might contain utf-8 or utf-16 LE so we might
            " want to try them first.
            set fileencodings=ucs-bom,utf-8,utf-16le,cp1252,iso-8859-15
          endif
        endif
    " }

"设置缩进有三个取值cindent(c风格)、smartindent(智能模式，其实不觉得有什么智能)、autoindent(简单的与上一行保持一致)
set cindent
"在windows版本中vim的退格键模式默认与vi兼容，与我们的使用习惯不太符合，下边这条可以改过来
set backspace=indent,eol,start

"用空格键替换制表符
set expandtab
"制表符占4个空格
set tabstop=4
"默认缩进4个空格大小
set shiftwidth=4
"把连续数量的空格视为一个制表符
set softtabstop=4

"显示行号
set number
set relativenumber
"增量式搜索
set incsearch
"高亮搜索
set hlsearch

set mouse=a
set virtualedit=onemore
set history=50
set hidden
set nobackup
"设置光标位于上次关闭时位置
autocmd BufReadPost *
            \ if line("'\"") > 1 && line("'\"") <= line("$") |
            \   exe "normal! g`\"" |
            \ endif

set tabpagemax=15
set showmode
set synmaxcol=2048
set showcmd
"总是显示状态栏
"set laststatus=2

"statusline
"set stl=%<
"set stl+=\ %f                     " Filename
"set stl+=\ %w%h%m%r                 " Options
"set stl+=\ %{getcwd()}          " Current dir
"set stl+=\ %{fugitive#statusline()} " Git Hotness
"set stl+=%=%p%%  " Right aligned file nav info
"set stl+=\ %y            " Filetype
"set stl+=\[%{(&fenc==\"\")?&enc:&fenc}%{(&bomb?\",BOM\":\"\")}]


    if has('statusline')
        set laststatus=2 "总是显示状态栏

        " Broken down into easily includeable segments
        set statusline=%<%f\                     " Filename
        set statusline+=%w%h%m%r                 " Options


        set statusline+=\ [%{&ff}/%Y]            " Filetype
        set statusline+=\ [%{getcwd()}]          " Current dir
        set statusline+=%=%-14.(%l,%c%V%)\ %p%%  " Right aligned file nav info
    endif


set linespace=0                 " No extra spaces between rows

set winminheight=0
set wildmenu                    " Show list instead of just completing
set list

set listchars=tab:›\ ,trail:•,extends:#,nbsp:. " Highlight problematic whitespace

set wig+=*/target/*,*/build/*,*/tmp/*,*/node_modules/*
set wig+=*.so,*.o,*.a,*.obj,*.swp,*.pyc,*.pyo,*.git,*.svn,*.exe,*.dll
set wig+=*.jar,*.class

set nowrap                      " Do not wrap long lines

set pastetoggle=<F12>           " pastetoggle (sane indentation on pastes)

"下边这个很有用可以根据不同的文件类型执行不同的命令
autocmd FileType c,cpp,java,go,php,javascript autocmd BufWritePre <buffer> call StripTrailingWhitespace()
autocmd FileType python,xml,vim autocmd BufWritePre <buffer> call StripTrailingWhitespace()

let mapleader = ','

" fullscreen mode for GVIM and Terminal, need 'wmctrl' in you PATH
nnoremap <silent> <F11> :call system("wmctrl -ir " . v:windowid . " -b toggle,fullscreen")<CR>

inoremap kl <esc>
cnoremap kl <esc>

cnoremap %% <C-R>=expand('%:p:~:h').'/'<cr>
nmap <leader>ee :e %%
nmap <leader>sp :sp %%
nmap <leader>vs :vsp %%
nmap <leader>ta :tabe %%
nnoremap <leader>cd :lcd %:h<CR>

nnoremap <leader>/ :nohlsearch<CR>

"nmap <leader>md :!mkdir -p %:p:h<CR>
nnoremap <leader>ev :e $MYVIMRC<CR>
nnoremap <leader>sv :so $MYVIMRC<CR>

cmap w!! w !sudo tee % >/dev/null

"inoremap <leader>uu <C-r>=substitute(system("uuidgen"), '.$', '', 'g')<CR>

" Quickly select the text that was just pasted. This allows you to, e.g.,
" indent it after pasting.
noremap gV `[v`]

" Stay in visual mode when indenting. You will never have to run gv after
" performing an indentation.
vnoremap < <gv
vnoremap > >gv

" Make Y yank everything from the cursor to the end of the line. This makes Y
" act more like C or D because by default, Y yanks the current line (i.e. the
" same as yy).
noremap Y y$

" Make Ctrl-e jump to the end of the current line in the insert mode. This is
" handy when you are in the middle of a line and would like to go to its end
" without switching to the normal mode.
inoremap <C-e> <C-o>$

" Allows you to easily replace the current word and all its occurrences.
nnoremap <Leader>rc :%s/<<C-r><C-w>>/
vnoremap <Leader>rc y:%s/<C-r>"/

" Allows you to easily change the current word and all occurrences to something
" else. The difference between this and the previous mapping is that the mapping
" below pre-fills the current word for you to change.
nnoremap <Leader>cc :%s/<<C-r><C-w>>/<C-r><C-w>
vnoremap <Leader>cc y:%s/<C-r>"/<C-r>"

" Replace tabs with four spaces. Make sure that there is a tab character between
" the first pair of slashes when you copy this mapping into your .vimrc!
nnoremap <Leader>rts :%s/ /    /g<CR>

" Remove ANSI color escape codes for the edited file. This is handy when
" piping colored text into Vim.
"nnoremap <Leader>rac :%s/<C-v><Esc>[(d{1,2}(;d{1,2}){0,2})?[m|K]//g<CR>


" Functions {

" Initialize directories {
function! InitializeDirectories()

    let dir_list = {
                \ 'backup': 'backupdir',
                \ 'views': 'viewdir',
                \ 'undo': 'undodir',
                \ 'swap': 'directory' }

    for [dirname, settingname] in items(dir_list)
        let directory = $HOME . '/.vim/' . dirname . '/'

        if !isdirectory(directory)
            call mkdir(directory)
        endif

        if isdirectory(directory)
            let directory = substitute(directory, " ", "\\\\ ", "g")
            exec "set " . settingname . "=" . directory
        else
            echo "Warning: Unable to create backup directory: " . directory
            echo "Try: mkdir -p " . directory
        endif
    endfor
endfunction
"call InitializeDirectories()
" }

" Strip whitespace {
function! StripTrailingWhitespace()
    " Preparation: save last search, and cursor position.
    let _s=@/
    let l = line(".")
    let c = col(".")
    " do the business:
    %s/\s\+$//e
    " clean up: restore previous search history, and cursor position
    let @/=_s
    call cursor(l, c)
endfunction
" }
