# neovimをインストールしてdein, deniteの導入まで

## 環境
mac os 10.13.2

## python3のインストール

```ShellScript
# install
brew install python3

# PATH_setting (bashやらzshの設定ファイルに記述
export PYTHONPATH=python3:pip3
```


## neovimのインストール

```
pip3 install neovim
```

## deinのインストール

```
# get installer.sh
curl https://raw.githubusercontent.com/Shougo/dein.vim/master/bin/installer.sh > ./installer.sh

# run installer.sh
sh ./installer.sh ~/.cache/dein/. ; rm ./installer.sh
```
以下のようなことを`~/.config/nvim/init.vim`に書きます

```
"dein Scripts-----------------------------
if &compatible
  set nocompatible               " Be iMproved
endif

" Required:
set runtimepath+=~/.cache/dein/repos/github.com/Shougo/dein.vim

" Required:
if dein#load_state('~/.cache/dein')
  call dein#begin('~/.cache/dein')

  " Let dein manage dein
  " Required:
  call dein#add('~/.cache/dein/repos/github.com/Shougo/dein.vim')

  " Add or remove your plugins here:
  call dein#add('Shougo/neosnippet.vim')
  call dein#add('Shougo/neosnippet-snippets')
  call dein#load_toml('~/dein.toml')
  if !has('nvim')
   	call dein#add('roxma/nvim-yarp')
		call dein#add('roxma/vim-hug-neovim-rpc')
  endif

  " You can specify revision/branch/tag.
  call dein#add('Shougo/deol.nvim', { 'rev': 'a1b5108fd' })

  " Required:
  call dein#end()
  call dein#save_state()
endif

" Required:
filetype plugin indent on
syntax enable

" If you want to install not installed plugins on startup.
"if dein#check_install()
"  call dein#install()
"endif

"End dein Scripts-------------------------
```

package管理用dein.tomlを`~/`直下に置いています
dein.tomlは以下のような感じです．

```
[[plugins]]
repo = 'Shougo/dein.vim'
```

## deniteのインストール

dein.tomlに以下のように追加します

```
[[plugins]]
repo = 'Shougo/denite.nvim'
```

vimを起動して，`:call dein#update()`やら`:UpdateRemotePlugins`を打ってインストールしてあげます

`:Denite grep`などと打ってDeniteが起動したら完了です

## 感想
ぼちぼち楽