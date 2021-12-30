# CentOS 下将 Vim 配置为 GO IDE

## 插件介绍

用到的插件如下：

==部分插件仓库已更新，注意更换==

- [vim-go](https://github.com/fatih/vim-go) 是针对go语言的vim插件。支持代码格式化、语法检查、语法高亮、调试等非常多的功能。
- [tagbar](https://github.com/preservim/tagbar) 用于方便查看代码结构。
- [nerdtree](https://github.com/preservim/nerdtree) 用于管理和查看代码目录结构。
- [YouCompleteMe](https://github.com/ycm-core/YouCompleteMe) 用于代码自动补全（实时跟踪代码补全）。

```bash
Plugin 'fatih/vim-go'
Plugin 'majutsushi/tagbar'
Plugin 'scrooloose/nerdtree'
" Plugin 'ycm-core/YouCompleteMe'
```

`.vimrc` 配置如下

```bash
set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

Plugin 'fatih/vim-go'
Plugin 'majutsushi/tagbar'
Plugin 'scrooloose/nerdtree'
" Plugin 'ycm-core/YouCompleteMe'

" The following are examples of different formats supported.
" Keep Plugin commands between vundle#begin/end.
" plugin on GitHub repo
Plugin 'tpope/vim-fugitive'
" plugin from http://vim-scripts.org/vim/scripts.html
" Plugin 'L9'
" Git plugin not hosted on GitHub
Plugin 'git://git.wincent.com/command-t.git'
" git repos on your local machine (i.e. when working on your own plugin)
Plugin 'file:///home/gmarik/path/to/plugin'
" The sparkup vim script is in a subdirectory of this repo called vim.
" Pass the path to set the runtimepath properly.
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" Install L9 and avoid a Naming conflict if you've already installed a
" different version somewhere else.
" Plugin 'ascenator/L9', {'name': 'newL9'}

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this lineset nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" The following are examples of different formats supported.
" Keep Plugin commands between vundle#begin/end.
" plugin on GitHub repo
Plugin 'tpope/vim-fugitive'
" plugin from http://vim-scripts.org/vim/scripts.html
" Plugin 'L9'
" Git plugin not hosted on GitHub
Plugin 'git://git.wincent.com/command-t.git'
" git repos on your local machine (i.e. when working on your own plugin)
" Plugin 'file:///home/gmarik/path/to/plugin'
" The sparkup vim script is in a subdirectory of this repo called vim.
" Pass the path to set the runtimepath properly.
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" Install L9 and avoid a Naming conflict if you've already installed a
" different version somewhere else.
" Plugin 'ascenator/L9', {'name': 'newL9'}

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line

" vim-go  golang
let g:go_autodetect_gopath = 1
let g:go_list_type = "quickfix"
let g:go_version_warning = 1
let g:go_highlight_types = 1
let g:go_highlight_fields = 1
let g:go_highlight_functions = 1
let g:go_highlight_function_calls = 1
let g:go_highlight_operators = 1
let g:go_highlight_extra_types = 1
let g:go_highlight_methods = 1
let g:go_highlight_generate_tags = 1
let g:godef_split=2

" NERDTree

" 打开和关闭NERDTree快捷键
map <F10> :NERDTreeToggle<CR>
" 设置宽度
let NERDTreeWinSize=25

" Tagbar
nmap <F9> :TagbarToggle<CR>
let g:tagbar_width=25
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
    \ 'ctagsbin'  : 'gotags',
    \ 'ctagsargs' : '-sort -silent'
    \ }
```

## 遇到的问题

执行 `:GoInstallBinaries` 失败

原因是在执行 `:GoInstallBinaries` 执行时会使用 `go get` 安装依赖包(依赖包在`~/.vim/bundle/vim-go/plugin/go.vim`中可以看到)，由于国内无法访问 `https://golang.org`，故会出现`io timeout`的情况，好在 google 已经将这些代码上传至 github 上。直接 clone 在本地再安装即可，以安装 `guru` 为例：

```bash
let s:packages = {
      \ 'asmfmt':        ['github.com/klauspost/asmfmt/cmd/asmfmt@latest'],
      \ 'dlv':           ['github.com/go-delve/delve/cmd/dlv@latest'],
      \ 'errcheck':      ['github.com/kisielk/errcheck@latest'],
      \ 'fillstruct':    ['github.com/davidrjenni/reftools/cmd/fillstruct@master'],
      \ 'godef':         ['github.com/rogpeppe/godef@latest'],
      \ 'goimports':     ['golang.org/x/tools/cmd/goimports@master'],
      \ 'golint':        ['golang.org/x/lint/golint@master'],
      \ 'revive':        ['github.com/mgechev/revive@latest'],
      \ 'gopls':         ['golang.org/x/tools/gopls@latest', {}, {'after': function('go#lsp#Restart', [])}],
      \ 'golangci-lint': ['github.com/golangci/golangci-lint/cmd/golangci-lint@latest'],
      \ 'staticcheck':   ['honnef.co/go/tools/cmd/staticcheck@latest'],
      \ 'gomodifytags':  ['github.com/fatih/gomodifytags@latest'],
      \ 'gorename':      ['golang.org/x/tools/cmd/gorename@master'],
      \ 'gotags':        ['github.com/jstemmer/gotags@master'],
      \ 'guru':          ['golang.org/x/tools/cmd/guru@master'],
      \ 'impl':          ['github.com/josharian/impl@master'],
      \ 'keyify':        ['honnef.co/go/tools/cmd/keyify@master'],
      \ 'motion':        ['github.com/fatih/motion@latest'],
      \ 'iferr':         ['github.com/koron/iferr@master'],
\ }
```

```bash
# 先在$GOPATH目录下本地建立golang.org\x
mkdir -p %GOPATH%/src/golang.org/x

# 将tools代码库clone到本地
git clone https://github.com/golang/tools.git %GOPATH%/src/golang.org/x/tools

# 安装guru
go install golang.org/x/tools/cmd/guru

# 安装成功后就可以在$GOPATH/bin 目录下看到guru的二进制文件
```

或者执行：

```bash
go install github.com/mgechev/revive@latest;go install golang.org/x/tools/gopls@latest;  go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest; go install honnef.co/go/tools/cmd/staticcheck@latest;go install github.com/fatih/gomodifytags@latest; go install golang.org/x/tools/cmd/gorename@master;go install github.com/jstemmer/gotags@master; go install golang.org/x/tools/cmd/guru@master; go install github.com/josharian/impl@master; go install honnef.co/go/tools/cmd/keyify@master; go install github.com/fatih/motion@latest; go install github.com/koron/iferr@master
```

==未完成==

安装插件 `YouCompleteMe`

- 将 vim 升级（重新安装），版本满足 `v8.1.2269` 及以上，参考[CentOS 安装vim8 + python3](https://blog.csdn.net/lxyoucan/article/details/114274117)
  - 使用以下命令（否则会报错：`YouCompleteMe unavailable: requires Vim compiled with Python (3.6.0+) support.`）

  ```bash
  ./configure --enable-python3interp=yes --enable-cscope --enable-fontset --with-python3-config-dir=/usr/lib/python3.6/config-3.6m-x86_64-linux-gnu --enable-python3interp=yes --with-python3-command=python3.6
  ```

  - 会遇到这个问题[ncurses not found when trying to build vim](https://stackoverflow.com/questions/34693071/ncurses-not-found-when-trying-to-build-vim)
- 下载插件，具体配置见[官网](https://github.com/ycm-core/YouCompleteMe)

## TODO

- [ ] 上述组件基本使用，常会用快捷键
- [ ] 代码实时自动补全
- [ ] NeoVim + SpaceVim 配置 Go 开发 IDE（极客时间-GO 语言项目开发实战推荐）

参考：

- [vim 配置golang环境](https://www.jianshu.com/p/8426cef1f4f5)
- [最好用的编辑器之一：Vim-Go环境搭建](https://zhuanlan.zhihu.com/p/51656877)
- [解决GitHub下载慢的有效方法](https://juejin.cn/post/6850037279587565582)
- [GO语言安装及vim-go的配置](https://blog.csdn.net/u014451076/article/details/52977223)
- [CentOS 安装vim8 + python3](https://blog.csdn.net/lxyoucan/article/details/114274117)
- [Go 开发 IDE 安装和配置](https://edgeplan.cn/2021/05/29/IAM%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/#%E4%BA%94%E3%80%81Go-%E5%BC%80%E5%8F%91-IDE-%E5%AE%89%E8%A3%85%E5%92%8C%E9%85%8D%E7%BD%AE)
