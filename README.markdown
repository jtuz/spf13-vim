# spf13-vim : jtuzp fork from Steve Francia's Vim Distribution

       _ _                                __ _ _____            _
      (_) |_ _   _ _____ __    ___ _ __  / _/ |___ /     __   _(_)_ __ ___
      | | __| | | |_  / '_ \  / __| '_ \| |_| | |_ \ ____\ \ / / | '_ ` _ \
      | | |_| |_| |/ /| |_) | \__ \ |_) |  _| |___) |_____\ V /| | | | | | |
     _/ |\__|\__,_/___| .__/  |___/ .__/|_| |_|____/       \_/ |_|_| |_| |_|
    |__/              |_|         |_|

spf13-vim is a distribution of vim plugins and resources for Vim, Gvim and [MacVim].

It is a good starting point for anyone intending to use VIM for development running equally well on Windows(through WSL), Linux, \*nix and Mac.

The distribution is completely customisable using a `~/.vimrc.local`, `~/.vimrc.bundles.local`, and `~/.vimrc.before.local` Vim RC files.

![spf13-vim image][spf13-vim-img]

Unlike traditional VIM plugin structure, which similar to UNIX throws all files into common directories, making updating or disabling plugins a real mess, spf13-vim 3 uses the 
[Plug] plugin management system to have a well organized vim directory (Similar to mac's app folders). Plug also ensures that the latest versions of your plugins are installed and makes it easy to keep them up to date.

This fork includes `.vim_plug.unplug` within `.vimrc` which has the UnPlug command, so that the user can remove unwanted plugins based from the default list, just like the old Vundle.
for example, to replace [Syntastic] with [ALE]

In `$HOME/.vimrc.bundles.local`
````
Plug 'w0rp/ALE'
UnPlug 'syntastic'
````

The user is to run
````
vim +PlugInstall +PlugClean!
````

Great care has been taken to ensure that each plugin plays nicely with others, and optional configuration has been provided for what we believe is the most efficient use.

Lastly (and perhaps, most importantly) It is completely cross platform. It works well on Windows(through WSL), Linux and OSX without any modifications or additional configurations. 
If you are using [MacVim] or Gvim additional features are enabled. So regardless of your environment just clone and run.

# Installation
## Requirements
To make all the plugins work, you need vim with lua.

**Note:** deoplete requires Neovim(0.3.0+, and of course **latest** is recommended) or Vim8.1 with python 3.6.1+ and timers enabled.

## Linux, \*nix, Mac OSX Installation

The easiest way to install spf13-vim is to use our [automatic installer](https://bit.ly/2H31LMx) by simply copying and pasting the following line into a terminal. 
This will install spf13-vim and backup your existing vim configuration. If you are upgrading from a prior version (before 4.0) this is also the recommended installation.

*Requires Git 1.7+ and Vim 7.3+*

```bash

    curl  https://bit.ly/2H31LMx -L > jtuzp-spf13-vim.sh && sh jtuzp-spf13-vim.sh
```

If you have a bash-compatible shell you can run the script directly:
```bash

    sh <(curl  https://bit.ly/2H31LMx -L)
```

For better compatibility between neovim and SPF-13 there are must be created the follow symlinks:

```sh
ln -s ~/.vimrc ~/.config/nvim/init.vim
ln -s ~/.vim ~/.config/nvim/.vim
ln -s ~/.vim/autoload ~/.local/share/nvim/site/autoload
```

## Installing on Windows

**WARNING!** this fork is not yet compatible whit the Windows installation, since Windows has WSL I highly recommend use the \*Nix installation through WSL

## Updating to the latest version
The simplest (and safest) way to update is to simply rerun the installer. It will completely and non destructively upgrade to the latest version.

```bash

    curl  https://bit.ly/2H31LMx -L -o - | sh

```

Alternatively you can manually perform the following steps. If anything has changed with the structure of the configuration you will need to create the appropriate symlinks.

```bash
    cd $HOME/to/spf13-vim/
    git pull
    vim +PlugInstall! +PlugClean +q
```

### Fork me on GitHub

I'm always happy to take pull requests from others. A good number of people are already [contributors] to [spf13-vim]. Go ahead and fork me.

# A highly optimized .vimrc config file

![spf13-vimrc image][spf13-vimrc-img]

The .vimrc file is suited to programming. It is extremely well organized and folds in sections.
Each section is labeled and each option is commented.

It fixes many of the inconveniences of vanilla vim including

 * A single config can be used across Windows, Mac and linux
 * Eliminates swap and backup files from littering directories, preferring to store in a central location.
 * Fixes common typos like :W, :Q, etc
 * Setup a solid set of settings for Formatting (change to meet your needs)
 * Setup the interface to take advantage of vim's features including
   * omnicomplete
   * relative line numbers
   * syntax highlighting
   * A better ruler & status line
   * & more
 * Configuring included plugins

## Customization

Create `~/.vimrc.local` and `~/.gvimrc.local` for any local
customizations.

For example, to override the default color schemes:

```bash
    echo colorscheme srcery  >> ~/.vimrc.local
```

### Before File

Create a `~/.vimrc.before.local` file to define any customizations
that get loaded *before* the spf13-vim `.vimrc`.

For example, to prevent autocd into a file directory:
```bash
    echo let g:spf13_no_autochdir = 1 >> ~/.vimrc.before.local
```
For a list of available spf13-vim specific customization options, look at the `~/.vimrc.before` file.


### Fork Customization

There is an additional tier of customization available to those who want to maintain a
fork of spf13-vim specialized for a particular group. These users can create `.vimrc.fork`
and `.vimrc.bundles.fork` files in the root of their fork.  The load order for the configuration is:

1. `.vimrc.before` - spf13-vim before configuration
2. `.vimrc.before.fork` - fork before configuration
3. `.vimrc.before.local` - before user configuration
4. `.vimrc.bundles` - spf13-vim bundle configuration
5. `.vimrc.bundles.fork` - fork bundle configuration
6. `.vimrc.bundles.local` - local user bundle configuration
6. `.vimrc` - spf13-vim vim configuration
7. `.vimrc.fork` - fork vim configuration
8. `.vimrc.local` - local user configuration

See `.vimrc.bundles` for specifics on what options can be set to override bundle configuration. See `.vimrc.before` for specifics
on what options can be overridden. Most vim configuration options should be set in your `.vimrc.fork` file, bundle configuration
needs to be set in your `.vimrc.bundles.fork` file.

You can specify the default bundles for your fork using `.vimrc.before.fork` file. Here is how to create an example `.vimrc.before.fork` file
in a fork repo for the default bundles.
```bash
    echo let g:spf13_bundle_groups=[\'general\', \'programming\', \'misc\', \'youcompleteme\'] >> .vimrc.before.fork
```
Once you have this file in your repo, only the bundles you specified will be installed during the first installation of your fork.

You may also want to update your `README.markdown` file so that the `bootstrap.sh` link points to your repository and your `bootstrap.sh`
file to pull down your fork.

For an example of a fork of spf13-vim that provides customization in this manner see [taxilian's fork](https://github.com/taxilian/spf13-vim).

### Easily Editing Your Configuration

`<Leader>ev` opens a new tab containing the .vimrc configuration files listed above. This makes it easier to get an overview of your
configuration and make customizations.

`<Leader>sv` sources the .vimrc file, instantly applying your customizations to the currently running vim instance.

These two mappings can themselves be customized by setting the following in .vimrc.before.local:
```bash
let g:spf13_edit_config_mapping='<Leader>ev'
let g:spf13_apply_config_mapping='<Leader>sv'
```

# Plugins

spf13-vim contains a curated set of popular vim plugins, colors, snippets and syntaxes. Great care has been made to ensure that these plugins play well together and have optimal configuration.

## Adding new plugins

Create `~/.vimrc.bundles.local` for any additional bundles.

To add a new bundle, just add one line for each bundle you want to install. The line should start with the word "Plug" followed by a string of either the vim.org project 
name or the githubusername/githubprojectname. For example, the github project [spf13/vim-colors](https://github.com/spf13/vim-colors) can be added with the following command

```bash
    echo Plug \'spf13/vim-colors\' >> ~/.vimrc.bundles.local
```

Once new plugins are added, they have to be installed.

```bash
    vim +PlugInstall! +PlugClean +q
```

## Removing (disabling) an included plugin

Create `~/.vimrc.local` if it doesn't already exist.

Add the UnPlug command to this line. It takes the same input as the Plug line, so simply copy the line you want to disable and add 'Un' to the beginning.

For example, disabling the 'AutoClose' and 'scrooloose/syntastic' plugins

```bash
    echo UnPlug \'AutoClose\' >> ~/.vimrc.bundles.local
    echo UnPlug \'scrooloose/syntastic\' >> ~/.vimrc.bundles.local
```

**Remember to run ':PlugClean!' after this to remove the existing directories**


Here are a few of the plugins:


## [Undotree]

If you undo changes and then make a new change, in most editors the changes you undid are gone forever, as their undo-history is a simple list.
Since version 7.0 vim uses an undo-tree instead. If you make a new change after undoing changes, a new branch is created in that tree.
Combined with persistent undo, this is nearly as flexible and safe as git ;-)

Undotree makes that feature more accessible by creating a visual representation of said undo-tree.

**QuickStart** Launch using `<Leader>u`.

## [NERDTree]

NERDTree is a file explorer plugin that provides "project drawer"
functionality to your vim editing.  You can learn more about it with
`:help NERDTree`.

**QuickStart** Launch using `<leader>ft`.

**Customizations**:

* Use `<leader>ft` to toggle NERDTree
* Use `<leader>e` or `<leader>nt` to load NERDTreeFind which opens NERDTree where the current file is located.
* Hide clutter ('\.pyc', '\.git', '\.hg', '\.svn', '\.bzr')
* Treat NERDTree more like a panel than a split.

## [FZF]
FZF replaces the [ctrlp] plugin with a fuzzy finder and asynchronous plugin. It provides an intuitive 
and fast mechanism to load files from the file system (with regex and fuzzy find), from open buffers, and from recently used files, and many
options with a great experience while you are using FZF within vim.

![spf13-vim image][spf13-vim-fzf]

**QuickStart** Launch using `<leader>ff`.

## [Surround]

This plugin is a tool for dealing with pairs of "surroundings."  Examples
of surroundings include parentheses, quotes, and HTML tags.  They are
closely related to what Vim refers to as text-objects.  Provided
are mappings to allow for removing, changing, and adding surroundings.

Details follow on the exact semantics, but first, consider the following
examples.  An asterisk (*) is used to denote the cursor position.

      Old text                  Command     New text ~
      "Hello *world!"           ds"         Hello world!
      [123+4*56]/2              cs])        (123+456)/2
      "Look ma, I'm *HTML!"     cs"<q>      <q>Look ma, I'm HTML!</q>
      if *x>3 {                 ysW(        if ( x>3 ) {
      my $str = *whee!;         vllllS'     my $str = 'whee!';

For instance, if the cursor was inside `"foo bar"`, you could type
`cs"'` to convert the text to `'foo bar'`.

There's a lot more, check it out at `:help surround`

## [NERDCommenter]

NERDCommenter allows you to wrangle your code comments, regardless of
filetype. View `help :NERDCommenter` or checkout my post on [NERDCommenter](http://spf13.com/post/vim-plugins-nerd-commenter).

**QuickStart** Toggle comments using `<Leader>c<space>` in Visual or Normal mode.

## [deoplete]
Deoplete is the abbreviation of "dark powered neo-completion". It provides an extensible and asynchronous 
completion framework for neovim/Vim8. Deoplete replaces the old Neocomplete plugin, this plugin also has support for snippets
It can complete simulatiously from the dictionary, buffer, omnicomplete and snippet.

**QuickStart** Just start typing, it will autocomplete where possible

**Customizations**:

 * Automatically present the autocomplete menu
 * Support tab and enter for autocomplete
 * `<C-k>` for completing snippets using [Neosnippet](https://github.com/Shougo/neosnippet.vim).

![neocomplete image][autocomplete-img]

## [ALE]

ALE (Asynchronous Lint Engine) is a plugin providing linting (syntax checking and semantic errors) in NeoVim 0.2.0+ and Vim 8 while 
you edit your text files, and acts as a Vim [Language Server Protocol client](https://langserver.org/).
by default language server feature is disabled, to enable you should set this into your `.vimrc.local` settings.

```
let g:ale_disable_lsp = 0
```

## [AutoClose]

AutoClose does what you expect. It's simple, if you open a bracket, paren, brace, quote,
etc, it automatically closes it. It handles curlys correctly and doesn't get in the
way of double curlies for things like jinja and twig.

## [Fugitive]

Fugitive adds pervasive git support to git directories in vim. For more
information, use `:help fugitive`

Use `:Gstatus` to view `git status` and type `-` on any file to stage or
unstage it. Type `p` on a file to enter `git add -p` and stage specific
hunks in the file.

Use `:Gdiff` on an open file to see what changes have been made to that
file

**QuickStart** `<leader>gs` to bring up git status

**Customizations**:

 * `<leader>gs` :Gstatus<CR>
 * `<leader>gd` :Gdiff<CR>
 * `<leader>gc` :Gcommit<CR>
 * `<leader>gb` :Gblame<CR>
 * `<leader>gl` :Glog<CR>
 * `<leader>gp` :Git push<CR>
 * `<leader>gw` :Gwrite<CR>
 * :Git ___ will pass anything along to git.

![fugitive image][fugitive-img]

## [PIV]

The most feature complete and up to date PHP Integration for Vim with proper support for PHP 5.3+ including latest syntax, functions, better fold support, etc.

PIV provides:

 * PHP 5.3 support
 * Auto generation of PHP Doc (,pd on (function, variable, class) definition line)
 * Autocomplete of classes, functions, variables, constants and language keywords
 * Better indenting
 * Full PHP documentation manual (hit K on any function for full docs)

![php vim itegration image][phpmanual-img]

## [Tabularize]

Tabularize lets you align statements on their equal signs and other characters

**Customizations**:

 * `<Leader>a= :Tabularize /=<CR>`
 * `<Leader>a: :Tabularize /:<CR>`
 * `<Leader>a:: :Tabularize /:\zs<CR>`
 * `<Leader>a, :Tabularize /,<CR>`
 * `<Leader>a<Bar> :Tabularize /<Bar><CR>`

## [Tagbar]

spf13-vim includes the Tagbar plugin. This plugin requires exuberant-ctags and will automatically generate tags for your open files. It also provides a panel to navigate easily via tags

**QuickStart** `CTRL-]` while the cursor is on a keyword (such as a function name) to jump to its definition.

**Customizations**: spf13-vim binds `<Leader>tt` to toggle the tagbar panel

![tagbar image][tagbar-img]

**Note**: For full language support, run `brew install ctags` to install
exuberant-ctags.

**Tip**: Check out `:help ctags` for information about VIM's built-in
ctag support. Tag navigation creates a stack which can traversed via
`Ctrl-]` (to find the source of a token) and `Ctrl-T` (to jump back up
one level).

## [EasyMotion]

EasyMotion provides an interactive way to use motions in Vim.

It quickly maps each possible jump destination to a key allowing very fast and
straightforward movement.

**QuickStart** EasyMotion is triggered using the normal movements, but prefixing them with `<leader><leader>`

For example this screen shot demonstrates pressing `,,w`

![easymotion image][easymotion-img]

## [Airline]

Airline provides a lightweight themable statusline with no external dependencies. By default this configuration uses the symbols `‹` and `›` as separators for different statusline sections but can be configured to use the same symbols as [Powerline]. An example first without and then with powerline symbols is shown here:

![airline image][airline-img]

To enable powerline symbols first install one of the [Powerline Fonts] or patch your favorite font using the provided instructions. Configure your terminal, MacVim, or Gvim to use the desired font. Finally add `let g:airline_powerline_fonts=1` to your `.vimrc.before.local`.

## Additional Syntaxes

spf13-vim ships with a few additional syntaxes:

* Markdown (bound to \*.markdown, \*.md, and \*.mk)
* Twig
* Git commits (set your `EDITOR` to `mvim -f`)

## Snippets

It also contains a very complete set of [snippets](https://github.com/spf13/snipmate-snippets) for use with snipmate or [deoplete].


# Intro to VIM

Here's some tips if you've never used VIM before:

## Tutorials

* Type `vimtutor` into a shell to go through a brief interactive
  tutorial inside VIM.
* Read the slides at [VIM: Walking Without Crutches](https://walking-without-crutches.heroku.com/#1).

## Modes

* VIM has two (common) modes:
  * insert mode- stuff you type is added to the buffer
  * normal mode- keys you hit are interpreted as commands
* To enter insert mode, hit `i`
* To exit insert mode, hit `<ESC>`

## Useful commands

* Use `:q` to exit vim
* Certain commands are prefixed with a `<Leader>` key, which by default maps to `\`.
  Spf13-vim uses `let mapleader = ","` to change this to `,` which is in a consistent and
  convenient location.
* Keyboard [cheat sheet](http://www.viemu.com/vi-vim-cheat-sheet.gif).

[![Analytics](https://ga-beacon.appspot.com/UA-7131036-5/spf13-vim/readme)](https://github.com/igrigorik/ga-beacon)
[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/spf13/spf13-vim/trend.png)](https://bitdeli.com/free "Bitdeli Badge")


[Git]:http://git-scm.com
[Curl]:http://curl.haxx.se
[Vim]:http://www.vim.org/download.php#pc
[MacVim]:http://code.google.com/p/macvim/
[spf13-vim]:https://github.com/jtuz/spf13-vim
[contributors]:https://github.com/jtuzp/spf13-vim/contributors

[Plug]:https://github.com/junegunn/vim-plug
[PIV]:https://github.com/spf13/PIV
[NERDCommenter]:https://github.com/scrooloose/nerdcommenter
[Undotree]:https://github.com/mbbill/undotree
[NERDTree]:https://github.com/scrooloose/nerdtree
[FZF]:https://github.com/junegunn/fzf
[solarized]:https://github.com/altercation/vim-colors-solarized
[deoplete]:https://github.com/Shougo/deoplete.nvim
[Fugitive]:https://github.com/tpope/vim-fugitive
[Surround]:https://github.com/tpope/vim-surround
[Tagbar]:https://github.com/majutsushi/tagbar
[Syntastic]:https://github.com/scrooloose/syntastic
[ALE]:https://github.com/w0rp/ale
[vim-easymotion]:https://github.com/Lokaltog/vim-easymotion
[Matchit]:http://www.vim.org/scripts/script.php?script_id=39
[Tabularize]:https://github.com/godlygeek/tabular
[EasyMotion]:https://github.com/Lokaltog/vim-easymotion
[Airline]:https://github.com/bling/vim-airline
[Powerline]:https://github.com/lokaltog/powerline
[Powerline Fonts]:https://github.com/Lokaltog/powerline-fonts
[AutoClose]:https://github.com/spf13/vim-autoclose

[spf13-vim-img]:https://i.imgur.com/xBqLjA6.png
[spf13-vimrc-img]:https://i.imgur.com/0VrkBcv.png
[spf13-vim-fzf]:https://i.imgur.com/WQjWcPt.png
[autocomplete-img]:https://i.imgur.com/JF5NOG6.png
[tagbar-img]:https://i.imgur.com/cjbrC.png
[fugitive-img]:https://i.imgur.com/3FpEf27.png
[nerdtree-img]:https://i.imgur.com/9xIfu.png
[phpmanual-img]:https://i.imgur.com/c0GGP.png
[easymotion-img]:https://i.imgur.com/ZsrVL.png
[airline-img]:https://i.imgur.com/D4ZYADr.png
