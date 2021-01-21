# vim/neovim editor

Vim is one of the most famous text editors in the nix world. It presents a simple and clean interface to it's user and is universally available on all Unix-like operating systems whether it's a minimal headless server or a full blown-up modern desktop. It's most attractive feature, specially for the devs & hackers, is the keyboard-centric approach that gives a time efficient workflow. It's other power feature is the ability to enhance and automate itself through scripting & plug-ins.

Above mentioned features also give Vim one of the steepest learning curves the text editor world. This document is written as an introduction to Vim for the newbies. We outline some basic commands from the default configuration and an overview of some famous plug-ins from a programmer/developer's point of view.



## Default Commands

Some useful commands from the default configuration are as follows:

| command / key                  | mode | description                                                  |
| ------------------------------ | ---- | ------------------------------------------------------------ |
| :help [keyword]                |      | Performs a search of help documentation                      |
| :e [file]                      |      | Opens a file                                                 |
| :w                             |      | Saves the file you are working on                            |
| :w [filename]                  |      | save your file with the name                                 |
| :wq                            |      | Save your file and close Vim                                 |
| :q!                            |      | Quit without first saving the file                           |
| **MOVEMENT**                   |      |                                                              |
| h/l                            | n    | Moves the cursor to the left/right                           |
| j/k                            | n    | Moves the cursor up/down one line                            |
| H                              | n    | Puts the cursor at the top of the screen                     |
| M                              | n    | Puts the cursor in the middle of the screen                  |
| L                              | n    | Puts the cursor at the bottom of the screen                  |
| w                              |      | Puts the cursor at the start of the next word                |
| b                              |      | Puts the cursor at the start of the previous word            |
| e                              |      | Puts the cursor at the end of a word                         |
| O                              |      | Places the cursor at the beginning of a line                 |
| $                              |      | Places the cursor at the end of a line                       |
| )                              |      | Takes you to the start of the next sentence                  |
| (                              |      | Takes you to the start of the previous sentence              |
| }                              |      | Takes you to the start of the next paragraph or block of text |
| {                              |      | Takes you to the start of the previous paragraph or block of text |
| C-f                            |      | Takes you one page forward                                   |
| C-b                            |      | Takes you one page back                                      |
| gg                             |      | Places the cursor at the start of the file                   |
| G                              |      | Places the cursor at the end of the file                     |
| #                              |      | takes you to the line specified (#: line number.)            |
| **Editing**                    |      |                                                              |
| a                              |      | Append text after the cursor [count] times.                  |
| A                              |      | Append text at the end of the line [count] times.            |
| i                              |      | Insert text before the cursor [count] times.                 |
| I                              |      | Insert text before the first non-blank in the line [count] times. |
| yy                             |      | Copies a line. `5yy` copies 5 lines                          |
| yw                             |      | Copies a word                                                |
| y$                             |      | Copies from cursor to the end of a line                      |
| v                              |      | Highlight one character at a time                            |
| V                              |      | Highlights one line                                          |
| p                              |      | Paste whatever has been copied to the unnamed register       |
| d                              |      | Deletes highlighted text                                     |
| dd                             |      | Deletes a line of text. `5dd` deletes 5 lines from cursor position |
| dw                             |      | Deletes a word                                               |
| D                              |      | Deletes everything from cursor to the end of the line        |
| d0                             |      | Deletes everything from cursor to the beginning of the line  |
| dgg                            |      | Deletes everything from cursor to the beginning of the file  |
| dG                             |      | Deletes everything from cursor to the end of the file        |
| x                              |      | Deletes a single character                                   |
| u                              |      | Undo the last operation; `u#` will undo multiple actions     |
| C-r                            |      | Redo last undo                                               |
| .                              |      | Repeat the last action                                       |
| o                              | n    | Insert a new line below                                      |
| O                              | n    | Insert a new line above                                      |
| **Searching**                  |      |                                                              |
| /[keyword]                     |      | Searches forward for the *keyword* in the document           |
| ?[keyword]                     |      | Searches backwards for the *keyword* in the document         |
| n                              |      | Searches again *in the direction* of your last search        |
| N                              |      | Searches again in the opposite direction of your last search |
| :%s/[pattern]/[replacement]/g  |      | This replaces all occurrences of a pattern without confirming each one |
| :%s/[pattern]/[replacement]/gc |      | Replaces all occurrences of a pattern and confirms each one  |
| **Multiple files**             |      |                                                              |
| :bn                            | n    | Switch to next buffer                                        |
| :bp                            | n    | Switch to previous buffer                                    |
| :bd                            | n    | Close a buffer                                               |
| :sp [filename]                 | n    | Opens a new file and splits your screen horizontally         |
| :vsp [filename]                | n    | Opens a new file and splits your screen vertically           |
| :ls                            | n    | Lists all open buffers                                       |
| <`C-w`>s                       | n    | Split windows horizontally                                   |
| <`C-w`>v                       | n    | Split windows vertically                                     |
| <`C-w`>w                       | n    | Switch between windows                                       |
| <`C-w`>q                       | n    | Quit a window                                                |
| <`C-w`>h / <`C-w`>l            | n,i  | Moves your cursor to the window to the left/right            |
| <`C-w`>j / <`C-w`>k            | n,i  | Moves your cursor to the window below/above                  |
| **Marking Text**               |      |                                                              |
| v                              | v    | starts visual mode, then select a range of text with hjkl    |
| V                              | v    | starts linewise visual mode                                  |
| C-v                            | v    | starts visual block mode (selects columns)                   |
| ab                             | v    | a block with ()                                              |
| aB                             | v    | a block with {}                                              |
| ib                             | v    | inner block with ()                                          |
| iB                             | v    | inner block with {}                                          |
| aw                             | v    | mark a word                                                  |
| Esc                            | v    | exit visual mode                                             |
| *For selected text*            |      |                                                              |
| d                              | v    | delete marked text                                           |
| y                              | v    | yank (copy) marked text                                      |
| >                              | v    | shift text right                                             |
| <                              | v    | shift text left                                              |
| ~                              | v    | swap case (upper or lower)                                   |
| **Tab pages**                  |      |                                                              |
| :tabedit file                  |      | opens a new tab and will take you to edit "file"             |
| gt                             |      | move to the next tab                                         |
| gT                             |      | move to the previous tab                                     |
| #gt                            |      | move to a specific tab number #                              |
| :tabs                          |      | list all tabs                                                |
| :tabclose                      |      | close a single tab                                           |



## Custom Commands



### Line numbering

Vim/Neovim gives four possible states for line numbering:

- no numbers
- absolute line numbers
- relative line numbers (zero on current  line).
- relative line numbers with absolute number on current  line.

Toggle all four settings, **no numbers** → **absolute** → **relative** → **relative with absolute on cursor line**:

```
:exe 'set nu!' &nu ? 'rnu!' : ''
```

Toggle between no **numbers** → **absolute** → **relative**:
```
:let [&nu, &rnu] = [&nu+&rnu==0, &nu]
```

Toggle between **no numbers** → **absolute** → **relative with absolute on cursor line**:
```
:let [&nu, &rnu] = [!&rnu, &nu+&rnu==1]
```

To map one of these commands to a key combination:

```
nnoremap <silent> <F6> :let [&nu, &rnu] = [!&rnu, &nu+&rnu==1]<CR>
```



## Plug-ins

Vim functionality can be immensely improved by adding plug-ins. A list of some useful plug-ins, that turn Vim into an IDE, are as follows:

- sheerun/vim-polyglot: Syntax Highlighting
- jiangmiao/auto-pairs: Bracket pairing
- neoclide/coc.nvim: Conquerer of Completion prediction & intellisense
- joshdick/onedark.vim: Theme
- vim-airline/vim-airline: Status/Tabline
- vim-airline/vim-airline-themes: Airline theme
- kevinhwang91/rnvimr: `ranger` file explorer integration
- junegunn/fzf.vim: `fzf` fuzzy search
- airblade/vim-rooter: Adjust file explorer path to VCS project root
- tpope/vim-commentary: Toggle comments
- norcalli/nvim-colorizer.lua: Shows color codes in same colored background
- junegunn/rainbow_parentheses.vim: Different color brackets in each nested scope.
- mhinz/vim-startify: Better project management with a fancy start screen
- mhinz/vim-signify: Show git hunks in vim gutter
- tpope/vim-fugitive: Git command wrapper for vim
- tpope/vim-rhubarb: Browse repo, insert issue # & urls
- junegunn/gv.vim: Git commit browser
- mattn/vim-gist: Easily create gists
- liuchengxu/vim-which-key: Pop up key-binding  hints on pressing `leader` key
- voldikss/vim-floaterm: A floating terminal for Neovim
- honza/vim-snippets: A repo of snipMate & UltiSnip snippets used with coc-snippets
- mattn/emmet-vim: Expand abbreviations for (x)HTML/CSS
- AndrewRadev/tagalong.vim: Rename closing HTML/XML tags when editing opening ones (vice versa)
- junegunn/goyo.vim: Distraction-free writing, zend mode

Plug-ins yet to be tested are:

- metakirby5/codi.vim: Interactive code
- skywind3000/asynctasks.vim: handle building/running/testing/deploying tasks
- skywind3000/asyncrun.vim: compile & run your code within vim
- turbio/bracey.vim: Live server



Check vim/neovim IDE configuration [nvim code](https://github.com/cod3g3nki/nvim) that uses these plug-ins.



### Emmet

According to the README:

> [emmet-vim](http://mattn.github.com/emmet-vim) is a vim plug-in which provides support for expanding abbreviations similar to [emmet](http://emmet.io/).

Tutorial for [emmet-vim](http://mattn.github.com/emmet-vim) is given by it's developer [mattn](mailto:mattn.jp@gmail.com) on the GitHub. The default `leader` key for emmet is `<C-Y>`.

1. Expand an Abbreviation

  Type the abbreviation as `div>p#foo$*3>a` and type '`<c-y>,`'.

  ```html
  <div>
      <p id="foo1">
          <a href=""></a>
      </p>
      <p id="foo2">
          <a href=""></a>
      </p>
      <p id="foo3">
          <a href=""></a>
      </p>
  </div>
  ```

2. Wrap with an Abbreviation

  Write as below.

  ```
  test1
  test2
  test3
  ```

  Then do visual select(line wise) and type '`<c-y>,`'.
  Once you get to the '`Tag:`' prompt, type '`ul>li*`'.

  ```html
  <ul>
      <li>test1</li>
      <li>test2</li>
      <li>test3</li>
  </ul>
  ```

  If you type a tag, such as 'blockquote', then you'll see the following:

  ```
  <blockquote>
      test1
      test2
      test3
  </blockquote>
  ```

3. Balance a Tag Inward

  type '`<c-y>d`' in insert mode.

4. Balance a Tag Outward

  type '`<c-y>D`' in insert mode.

5. Go to the Next Edit Point

  type '`<c-y>n`' in insert mode.

6. Go to the Previous Edit Point

  type '`<c-y>N`' in insert mode.

7. Update an `<img>`’s Size

  Move cursor to the img tag.

  ```html
  <img src="foo.png" />
  ```

  Type '`<c-y>i`' on img tag

  ```html
  <img src="foo.png" width="32" height="48" />
  ```

8. Merge Lines

  select the lines, which include '`<li>`'

  ```html
  <ul>
  	<li class="list1"></li>
  	<li class="list2"></li>
  	<li class="list3"></li>
  </ul>
  ```

  and then type '`<c-y>m`'

  ```html
  <ul>
  	<li class="list1"></li><li class="list2"></li><li class="list3"></li>
  </ul>
  ```

9. Remove a Tag

  Move cursor in block

  ```html
  <div class="foo">
  	<a>cursor is here</a>
  </div>
  ```

  Type '`<c-y>k`' in insert mode.

  ```html
  <div class="foo">

  </div>
  ```

  And type '`<c-y>k`' in there again.

  ```

  ```

10. Split/Join Tag

    Move the cursor inside block

  ```html
  <div class="foo">
  	cursor is here
  </div>
  ```

  Type '`<c-y>j`' in insert mode.

  ```html
  <div class="foo"/>
  ```

  And then type '`<c-y>j`' in there again.

  ```html
  <div class="foo">
  </div>
  ```

11. Toggle Comment

  Move cursor inside the block

  ```html
  <div>
  	hello world
  </div>
  ```

  Type '`<c-y>/`' in insert mode.

  ```html
  <!-- <div>
  	hello world
  </div> -->
  ```

  Type '`<c-y>/`' in there again.

  ```html
  <div>
  	hello world
  </div>
  ```

12. Make an anchor from a URL

  Move cursor to URL

  ```html
  http://www.google.com/
  ```

  Type '`<c-y>a`'

  ```html
  <a href="http://www.google.com/">Google</a>
  ```

13. Make some quoted text from a URL

  Move cursor to the URL

  ```html
http://github.com/
  ```

  Type '`<c-y>A`'

  ```html
  <blockquote class="quote">
  	<a href="http://github.com/">Secure source code hosting and collaborative development - GitHub</a><br />
  	<p>How does it work? Get up and running in seconds by forking a project, pushing an existing repository...</p>
  	<cite>http://github.com/</cite>
  </blockquote>
  ```

14. Installing emmet.vim for the language you are using:

  ```sh
cd ~/.vim
unzip emmet-vim.zip
  ```

  Or if you are using pathogen.vim:

  ```sh
cd ~/.vim/bundle # or make directory
unzip /path/to/emmet-vim.zip
  ```

  Or if you get the sources from the repository:

  ```sh
cd ~/.vim/bundle # or make directory
git clone http://github.com/mattn/emmet-vim.git
  ```

15. Enable emmet.vim for the language you using.

  You can customize the behavior of the languages you are using.

```sh
cat >> ~/.vimrc
```

  ```
let g:user_emmet_settings = {
    \  'php' : {
    \    'extends' : 'html',
    \    'filters' : 'c',
    \  },
    \  'xml' : {
    \    'extends' : 'html',
    \  },
    \  'haml' : {
    \    'extends' : 'html',
    \  },
    \}
  ```

