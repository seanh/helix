My [Helix](https://helix-editor.com/) editor config
===================================================

Notes on Helix
--------------

Rough notes from learning Helix. At some point I might turn these into a handful of blog posts. For now I have written [a post on Helix's language server features](https://www.seanh.cc/2025/10/13/helix-lsp-virtualenv/).

### Intro

TODO: Add some intro text about what Helix actually is, including the vision:
https://github.com/helix-editor/helix/blob/master/docs/vision.md
This Kakoune design document is also pretty relevant: https://kakoune.org/why-kakoune/why-kakoune.html

### Launching Helix

```shell-session
$ hx
```

By default it opens a scratch buffer.

TODO: `<files>`, `[:row[:column]]`, `+N`. 

TODO: `--config` / `-c` to launch with a different config file.

TODO: `--working-dir` / `-w` to launch with a different working directory.

### The command line

<kbd>:</kbd> enters Helix's command line, which is a line at the bottom of the editor where you can type commands.

One thing you can do from the command line in Helix is open a file by typing one of these commands followed by <kbd>Enter</kbd>:

* `:open <path>`, `:o <path>`, `:edit <path>` or `:e <path>`
  (they're all the same).

  `<path>` can be the path to an existing file to open an existing file,
  or it can be the path to a filename that doesn't exist yet to create a new
  file.

  `<path>` can also be the path to an (existing) directory and Helix will
  open the directory in its file picker (see below).

  Notice that Helix shows auto-complete suggestions for both the the command
  and the file path (you can use them with <kbd>Tab</kbd> and <kbd>Shift</kbd> + <kbd>Tab</kbd>).

  It also shows the docs for the currently selected command.

* If you want to open a new file but don't want to think of a filename or
  directory for it yet you can use `:new` or `:n` to open a new scratch buffer.
  You'll have to enter a path or filename if and when you want to save the file.

Another thing you can do from the command line is _save_ the file:

* `:write` or `:w`.
  Accepts optional file path as argument and supports path completion.
  You can change a file's name/path by doing `:w <new_path>`, but this will also leave a copy of the file behind at the previous path. `:move` / `:mv` will move the file without leaving a copy behind.
  * `:write!` or `:w!` to save and create subdirs
* `:write-quit` or `:wq` or `:x` to save changes and quit
* `:write-quit-all` or `:wqa` or `:xa`
* `:write-quit-all!` or `:wqa!` or `:xa!`

The command line is also how you quit and exit Helix, by entering one of these commands:

* `:quit` or `:q`.
  This will refuse to quit if you have any unsaved changes,
  you have to save them first.
* `:quit!` or `:q!`: quit and discard any unsaved changes.
* `:quit-all` or `:qa`
  `:q` actually only closes the current view, `:qa` closes all views.
* `:quit-all!` or `:qa!`

The command line is what's called a "prompt" in Helix.
There are other prompts as well, such as the one for entering a search term (<kbd>/</kbd>).
As well as <kbd>Enter</kbd>, <kbd>Tab</kbd> and <kbd>Shift</kbd> + <kbd>Tab</kbd>,
here are some more keybindings that work in all prompts ([docs](https://docs.helix-editor.com/keymap.html#prompt)):

* <kbd>Esc</kbd> or <kbd>Ctrl</kbd> + <kbd>c</kbd> to close the prompt without doing anything (e.g. close the command line without entering a command)
* <kbd>Ctrl</kbd> + <kbd>b</kbd> or <kbd>&larr;</kbd>:
  move the cursor back (left) by one character
* <kbd>Ctrl</kbd> + <kbd>f</kbd> or <kbd>&rarr;</kbd>:
  move the cursor _forward_ (right) by one character
* <kbd>Ctrl</kbd> + <kbd>&larr;</kbd> or <kbd>Alt</kbd> + <kbd>b</kbd>:
  move the cursor back (left) one _word_
* <kbd>Ctrl</kbd> + <kbd>&rarr;</kbd> or <kbd>Alt</kbd> + <kbd>f</kbd>:
  move the cursor _forward_ (right) one word
* <kbd>Ctrl</kbd> + <kbd>a</kbd> or <kbd>Home</kbd>:
  move the cursor to the start of the line
* <kbd>Ctrl</kbd> + <kbd>e</kbd> or <kbd>End</kbd>:
  move the cursor to the _end_ of the line
* <kbd>Backspace</kbd> or <kbd>Ctrl</kbd> + <kbd>h</kbd>: delete the previous character
* <kbd>Del</kbd> or <kbd>Ctrl</kbd> + <kbd>d</kbd>: delete the _next_ character
* <kbd>Ctrl</kbd> + <kbd>w</kbd>, <kbd>Alt</kbd> + <kbd>Backspace</kbd> or <kbd>Ctrl</kbd> + <kbd>Backspace</kbd>: delete the previous word
* <kbd>Alt</kbd> + <kbd>d</kbd>, <kbd>Alt</kbd> + <kbd>Del</kbd> or <kbd>Ctrl</kbd> + <kbd>Del</kbd>: delete the _next_ word
* <kbd>Ctrl</kbd> + <kbd>u</kbd>: delete to the start of the line
* <kbd>Ctrl</kbd> + <kbd>k</kbd>: delete to the _end_ of the line
* <kbd>Ctrl</kbd> + <kbd>s</kbd>:
  insert the word that's under the cursor in the document
  onto the command line
* <kbd>Ctrl</kbd> + <kbd>p</kbd> or <kbd>&uarr;</kbd>
  <kbd>Ctrl</kbd> + <kbd>n</kbd> or <kbd>&darr;</kbd>
  browse the history of previous commands in this editing session.
* <kbd>Ctrl</kbd> + <kbd>r</kbd> <kbd>&lt;ch&gt;</kbd>:
  insert the contents or register &lt;ch&gt; onto the command line.

When re-opening a prompt the previous command that was entered into the same prompt will be shown in faded text. Press <kbd>Enter</kbd> to accept the suggestion and enter the same command again.

We'll learn about many commands throughout these notes but here's a grab-bag if miscellaneous interesting commands that aren't mentioned elsewhere (see the [docs](https://docs.helix-editor.com/commands.html#typable-commands) for the full list):

* `:theme` to change the current theme (with tab-complete and theme preview)
* `:reload` / `:rl` and `:reload-all` / `:rla`
* `:read` / `:r` to read an exiting file from disk into the current buffer
* `:change-current-directory`, `:cd`
* `:show-directory`, `:pwd`
* `:character-info` / `:char` prints info about the character under the cursor (its unicode code etc)
* `:set-language` / `:lang` sets the language of the current buffer or shows the current language if no argument is given
* `:reflow` hard-wraps the current selection to the width given by the `text-width` option (default: `80`), or `:reflow <width>` to wrap it to a different width.
* `:reset-diff-change` / `:diffget` / `:diffg` resets the change under the cursor (git)

There's also a bunch of expansions you can use on the command line: https://docs.helix-editor.com/command-line.html#expansions

### Command palette

Instead of using the command line, many Helix commands can also be entered via a searchable command palette that you open with <kbd>Space</kbd> <kbd>?</kbd>.
See pickers below for keyboard shortcuts for the command palette.

Since the command palette shows the keybindings and docstring for each command it can actually be a useful way to discover or remind yourself about commands and key mappings.

The command palette only works for commands with no arguments or where the argument(s) have default values. Commands with required arguments actually do appear in the command palette but they don't work! For example try entering the `:move` command from the command palette, or `:open`, etc.

There are also some commands that you can use from the command palette that you _can't_ use from the command line. The command line only works for [typable commands](https://docs.helix-editor.com/commands.html#typable-commands) whereas the command palette works for both typable commmands (as long as they don't have any required arguments) and [static commands](https://docs.helix-editor.com/commands.html#static-commands).

As well as executing them via the command palette static commands can be mapped to keyboard shortcuts. Many static commands are mapped to default keyboard shortcuts, and you can add custom mappings ([docs](https://docs.helix-editor.com/remapping.html)) for those that aren't.

### Pickers

The command palette is an example of a "picker".
Helix has many pickers, many of which are opened via Helix's "space mode" ([docs](https://docs.helix-editor.com/keymap.html#space-mode)):

* Command palette: <kbd>Space</kbd> <kbd>?</kbd>
* File picker: <kbd>Space</kbd> <kbd>f</kbd> (LSP workspace root)
  or <kbd>Space</kbd> <kbd>F</kbd> (current working directory)
* File _explorer_: <kbd>Space</kbd> <kbd>e</kbd> (LSP workspace root)
  or <kbd>Space</kbd> <kbd>E</kbd> (current working directory)
* Buffer picker: <kbd>Space</kbd> <kbd>b</kbd> (see buffers below)
* Jumplist picker: <kbd>Space</kbd> <kbd>j</kbd> (see jumplist below)
* Symbol picker: <kbd>Space</kbd> <kbd>s</kbd> (for all symbols in the current file) or <kbd>Space</kbd> <kbd>S</kbd> (for symbols across the whole workspace), see my post about Helix's LSP support
* Diagnostics picker: <kbd>Space</kbd> <kbd>d</kbd> (file) or <kbd>Space</kbd> <kbd>D</kbd> (workspace), again see my post about Helix and LSPs)
* Changed file picker (via Helix's built-in git integration): <kbd>Space</kbd> <kbd>g</kbd>
* <kbd>Space</kbd> <kbd>'</kbd> opens the previously opened picker _and maintains its state_. Whereas <kbd>Space</kbd> <kbd>f</kbd> (for example) repeatedly opens a new file picker each time, with the state reset.

Like with prompts, when re-opening a picker the previous filter that was entered into the same picker will be shown in faded text. Press <kbd>Enter</kbd> to accept the suggestion and enter the same filter again. Unlike with prompts, in pickers only the previous filter is available: there's no history of filters.

TODO: Explain space mode.

TODO: Explain minor modes in general, of which space mode is an example. Minor modes usually have an accompanying popup menu of keyboard shortcuts that appears in the bottom left. And selecting a shortcut usually exits the minor mode immediately: they're one-shot modes.

Some keyboard shortcuts for pickers ([docs](https://docs.helix-editor.com/keymap.html#picker)):

* <kbd>Enter</kbd> choose the currently selected item
* <kbd>Alt</kbd> + <kbd>Enter</kbd> choose item without closing picker
* <kbd>Ctrl</kbd> + <kbd>s</kbd>: open item in horizontal split
* <kbd>Ctrl</kbd> + <kbd>v</kbd>: open item in _vertical_ split
* <kbd>Ctrl</kbd> + <kbd>t</kbd>: toggle preview pane
* <kbd>Tab</kbd>, <kbd>&darr;</kbd>, or <kbd>Ctrl</kbd> + <kbd>n</kbd>:
  select the next entry in the list
* <kbd>Shift</kbd> + <kbd>Tab</kbd>, <kbd>&uarr;</kbd>, or <kbd>Ctrl</kbd> + <kbd>p</kbd>:
  select the _previous_ entry in the list
* <kbd>Page Down</kbd> or <kbd>Ctrl</kbd> + <kbd>d</kbd>:
  move down a page in the list
* <kbd>Page Up</kbd> or <kbd>Ctrl</kbd> + <kbd>u</kbd>:
  move _up_ a page in the list
* <kbd>Home</kbd>: go to the first item in the list
* <kbd>End</kbd>: go to the last item in the list

Many of the keyboard shortcuts for prompts also work in pickers (but not all: some already have different bindings for pickers above):

* <kbd>Esc</kbd> or <kbd>Ctrl</kbd> + <kbd>c</kbd> to close the picker without doing anything
* <kbd>Ctrl</kbd> + <kbd>b</kbd> or <kbd>&larr;</kbd>:
  move the cursor back (left) by one character
* <kbd>Ctrl</kbd> + <kbd>f</kbd> or <kbd>&rarr;</kbd>:
  move the cursor _forward_ (right) by one character
* <kbd>Ctrl</kbd> + <kbd>&larr;</kbd> or <kbd>Alt</kbd> + <kbd>b</kbd>:
  move the cursor back (left) one _word_
* <kbd>Ctrl</kbd> + <kbd>&rarr;</kbd> or <kbd>Alt</kbd> + <kbd>f</kbd>:
  move the cursor _forward_ (right) one word
* <kbd>Ctrl</kbd> + <kbd>a</kbd>:
  move the cursor to the start of the line
* <kbd>Ctrl</kbd> + <kbd>e</kbd>:
  move the cursor to the _end_ of the line
* <kbd>Backspace</kbd> or <kbd>Ctrl</kbd> + <kbd>h</kbd>: delete the previous character
* <kbd>Del</kbd>: delete the _next_ character
* <kbd>Ctrl</kbd> + <kbd>w</kbd>, <kbd>Alt</kbd> + <kbd>Backspace</kbd> or <kbd>Ctrl</kbd> + <kbd>Backspace</kbd>: delete the previous word
* <kbd>Alt</kbd> + <kbd>d</kbd>, <kbd>Alt</kbd> + <kbd>Del</kbd> or <kbd>Ctrl</kbd> + <kbd>Del</kbd>: delete the _next_ word
* <kbd>Ctrl</kbd> + <kbd>k</kbd>: delete to the _end_ of the line
* <kbd>Ctrl</kbd> + <kbd>r</kbd> <kbd>&lt;ch&gt;</kbd>:
  insert the contents or register &lt;ch&gt; onto the command line.

Pickers use fuzzy-matching with fzf syntax: https://github.com/junegunn/fzf?tab=readme-ov-file#search-syntax

* `'wild` (with an opening `'` but no closing one) does an exact match rather than a fuzzy match
* `'wild'` (with both opening and closing `'`'s) does an exact word boundary match
* `^music` does a prefix match
* `.mp3$` does a suffix match
* `!fire` does an inverse match. This can be combined with `^` and `$`, for example: `!^music` or `!.mp3$`
* Helix does not support fzf's `|`

The global search picker uses regexes rather than fuzzy matching.

The workspace symbol picker works by passing search terms to your LSP.

You can match certain columns only e.g. "%path foo" or "%p foo".

### Moving the cursor and selection in normal mode

* <kbd>h</kbd>, <kbd>j</kbd>, <kbd>k</kbd>, <kbd>l</kbd>
  If you have soft-wrap enabled (`:toggle soft-wrap.enable`) then <kbd>gj</kbd> and <kbd>gk</kbd> move down and up by actual file lines rather than visual lines.
* <kbd>w</kbd> selects from the current cursor position to _the character after the next word ending_. If the cursor is in the middle of a word it'll go to the space following that word. If the cursor in in the whitespace between two words it'll go the space following the following word. Also <kbd>W</kbd>.
* <kbd>e</kbd> selects to the next word ending. Also <kbd>E</kbd>. If the cursor is on the last word on the line then <kbd>w</kbd> and <kbd>e</kbd> are the same.
* <kbd>b</kbd> and <kbd>B</kbd> are <kbd>e</kbd> and <kbd>E</kbd> backwards
* <kbd>;</kbd> collapses the selection,
  leaving the cursor in its new location.
  The selection also collapses if you move the cursor with <kbd>h</kbd>, <kbd>j</kbd>, <kbd>k</kbd> or <kbd>l</kbd>.
* <kbd>Alt</kbd> + <kbd>;</kbd>: flip the direction of the selection
  (swaps the "cursor" and "anchor")
  Useful when extending selections
  <kbd>Alt</kbd> + <kbd>:</kbd>
  (i.e. <kbd>Alt</kbd> + <kbd>Shift</kbd> + <kbd>;</kbd>)
  sets the direction of the selection to forward
* <kbd>f&lt;ch&gt;</kbd> selects up to and including the next &lt;ch&gt;,
  <kbd>t&lt;ch&gt;</kbd> selects up until but not including the next &lt;ch&gt;.
  <kbd>F</kbd> and <kbd>T</kbd> do the same backwards
* <kbd>Alt</kbd> + <kbd>.</kbd> repeats the last
  <kbd>f</kbd>, <kbd>t</kbd>, <kbd>F</kbd>, <kbd>T</kbd>, <kbd>[</kbd> or <kbd>]</kbd> motion.
* Two-character label jumping: <kbd>gw</kbd>.
  <kbd>Esc</kbd> or <kbd>Ctrl</kbd> + <kbd>c</kbd> to exit without jumping.
* <kbd>Page Up</kbd>, <kbd>Page Down</kbd>, <kbd>Ctrl</kbd> + <kbd>b</kbd>, <kbd>Ctrl</kbd> + <kbd>f</kbd>
* <kbd>Ctrl</kbd> + <kbd>u</kbd> and <kbd>Ctrl</kbd> + <kbd>d</kbd> move a half-page up and down
* There's a bunch of non-selecting cursor movements accessed by the <kbd>g</kbd> key (the **Goto window**):
  * Top of file: <kbd>gg</kbd>
  * End of file: <kbd>ge</kbd>
  * Line number: <kbd>2gg</kbd> or <kbd>2G</kbd>, or `:goto` / `:g`
  * Start of line: <kbd>gh</kbd> or <kbd>Home</kbd>
  * Go to first non-whitespace character on line: <kbd>gs</kbd>
  * End of line: <kbd>gl</kbd> or <kbd>End</kbd>
  * Go to column: <kbd>2g|</kbd>
  * Go to window top, center, bottom: <kbd>gt</kbd>, <kbd>gc</kbd>, <kbd>gb</kbd>
  * Go to site of last modification: <kbd>g.</kbd>

unimpaired.vim-style movements with <kbd>]</kbd> and <kbd>[</kbd>:

* <kbd>]d</kbd>, <kbd>[d</kbd>, <kbd>]D</kbd> and <kbd>]D</kbd> go to and select the next, previous, last and first diagnostic (see my post on LSP integration)
* <kbd>]g</kbd>, <kbd>[g</kbd>, <kbd>]G</kbd> and <kbd>]G</kbd> go to and select the next, previous, last and first change (git integration)
* <kbd>]p</kbd> and <kbd>[p</kbd> go to and select the next and previous paragraph

There's also a bunch of tree-sitter-based ones. I'm not sure how useful these are, many of them seem confusing and unpredictable:

* <kbd>]f</kbd> or <kbd>[f</kbd>:
  go to and select next or previous function
* <kbd>]t</kbd> or <kbd>[t</kbd>:
  go to and select next or previous type definition (e.g. class)
* <kbd>]a</kbd> or <kbd>[a</kbd>:
  go to and select next argument/parameter in a function/method signature or call
* <kbd>]c</kbd>: comment
* <kbd>]T</kbd>: test
* <kbd>]e</kbd> "pairing" (?)
* <kbd>Alt</kbd> + <kbd>n</kbd> and <kbd>Alt</kbd> + <kbd>p</kbd>
  go to the next and previous sibling in the syntax tree.
  For example if the cursor is on a method argument these will go to the next and previous method arguments (also stopping on the `,`'s between the arguments: those are siblings in the syntax tree too).
  A slightly confusing thing is that these will also move up and down the syntax tree once they run out of siblings. So for example if you're on the first argument to a method and press <kbd>Alt</kbd> + <kbd>p</kbd> again it'll move up to the `(` that opens the method signature or call, then to the method name, etc. If you're on the _last_ argument and press <kbd>Alt</kbd> + <kbd>n</kbd> again it'll move to the closing `)`, then the `:`, then to the method body, etc.
* <kbd>Alt</kbd> + <kbd>i</kbd> moves to the child node in the syntax tree, and <kbd>Alt</kbd> + <kbd>o</kbd> moves to the parent node.
* <kbd>Alt</kbd> + <kbd>a</kbd> selects all sibling nodes
* <kbd>Alt</kbd> + <kbd>I</kbd> selects all child nodes
* <kbd>Alt</kbd> + <kbd>e</kbd> moves to end of parent node
* <kbd>Alt</kbd> + <kbd>b</kbd> moves to start of parent node

### Counts

Counts can be added to all these motions e.g. <kbd>3w</kbd> or <kbd>3x</kbd>.

### The jump list

Whenever you make a "significant jump" (i.e. cursor movement) Helix remembers your cursor positions/selections from before the jump, adding them to its jumplist. You can use the jumplist to jump back to places where you've previously been/to previous selections, across all files in the current session. Keep in mind that it's not "the cursor position" that gets saved to the jumplist but "the selections".

<kbd>Ctrl</kbd> + <kbd>o</kbd> jumps back to the previous location in the jumplist, <kbd>Ctrl</kbd> + <kbd>i</kbd> jumps forward to the next position.

<kbd>Space</kbd> <kbd>j</kbd> opens a picker for the contents of the jumplist.

Things that add a new entry to the jumplist: changing files, search with <kbd>/</kbd>, commands that change the current selection(s) like <kbd>s</kbd>, <kbd>S</kbd>, <kbd>k</kbd> and <kbd>Alt</kbd> + <kbd>k</kbd> (see multiple selections below), jumping to the start of a file (<kbd>gg</kbd>) or to the end (<kbd>ge</kbd>), jumping to a line by number, LSP commands like goto-definition/declaration/type-definition/references, etc.

<kbd>Ctrl</kbd> + <kbd>s</kbd> manually saves the current position to the jump list.

### Select/extend mode

<kbd>v</kbd> enters select mode. <kbd>v</kbd> again or <kbd>Esc</kbd> exits.

In select mode movements that would just move the cursor in normal mode will now also extend the selection, e.g. <kbd>h</kbd>, <kbd>j</kbd> <kbd>k</kbd> <kbd>l</kbd>.

A good metaphor is to think of entering select mode as "dropping an anchor" (since "anchor" is Helix's word for the opposite end of a selection: the end that isn't the cursor). When in normal mode the anchor is onboard the ship and movement commands move both the anchor and the cursor (albeit the anchor seems to be trailing behind sometimes). Entering select mode drops the anchor at its current location.

Movements in normal mode would move the cursor and change the selection now also extend the selection, e.g. to select three words or lines: <kbd>v3w</kbd>.

Edit commands like <kbd>d</kbd> automatically exit select mode.

### Selecting lines

* <kbd>x</kbd> is kind of an odd-one-out in that you can use it repeatedly in normal mode to select multiple lines, you don't need to go into select mode. Counts also work with <kbd>x</kbd>. But there's no way to select one line, then move the selection to the next line, like you can with words.

* <kbd>x</kbd> on an empty line will select the _next_ line. This is because the cursor (conceived of as a single-character selection) already selects the entire empty line.

* <kbd>X</kbd> selects the whole current line and does nothing if the whole line is already selected. I think this exists just so that there's an "ensure entire current line is selected" command that doesn't have any potential side-effects (such as selecting the line below). Useful for macros and keybindings.

### Making edits from normal mode or select mode

You'll use insert mode (see below) when you want to enter new text into a file. But edits like deleting and changing things can be made directly from normal or select mode:

* <kbd>d</kbd>: delete selection.
  If no text is selected this just deletes the character under the cursor:
  think of a cursor as a single-character selection.
  On an empty line this deletes the entire line.
  To delete an entire non-empty line you need to select it with <kbd>x</kbd> or <kbd>X</kbd> then hit <kbd>d</kbd>.
* <kbd>r&lt;ch&gt;</kbd>:
  replace the character under the cursor with &lt;ch&gt;.
  If multiple characters are selected replaces them all with many &lt;ch&gt;'s.
* <kbd>&gt;</kbd> and <kbd>&lt;</kbd> indent and dedent the cursor line or all selected lines or all lines with cursors/selections on them
* <kbd>Ctrl</kbd> + <kbd>a</kbd> and <kbd>Ctrl</kbd> + <kbd>x</kbd> increment and decrement numbers, if the cursor is on a number.
* <kbd>~</kbd> toggles the case of the character under the cursor or of all characters. <kbd>\`</kbd> lowercases, <kbd>Alt</kbd> + <kbd>`</kbd> uppercases
* <kbd>Ctrl</kbd> + <kbd>c</kbd> or <kbd>Space</kbd> <kbd>c</kbd> comments or uncomments the current line or lines.
  For languages that have different line-comment and block-comment styles
  <kbd>Space</kbd> <kbd>C</kbd> block-comments the selected line(s).
* <kbd>J</kbd> joins the selected lines into one line.
  There's no "unjoin lines" command exactly,
  but `:reflow` will rewrap lines to the width given in the `text-width` option (80 by default), or <code>:reflow &lt;n&gt;</code> will reflow the selected text to width <code>&lt;n&gt;</code>,
  whereas `:format` or `:fmt` will reformat lines if you have a formatter configured for the language, or an LSP that supports code formatting.
* <kbd>]</kbd> <kbd>Space</kbd> inserts an empty line below the current line, <kbd>[</kbd> <kbd>Space</kbd> inserts one above

### Insert mode

* Entering insert mode: <kbd>i</kbd>, <kbd>a</kbd>, <kbd>I</kbd>, <kbd>A</kbd>, <kbd>o</kbd>, <kbd>O</kbd>
* <kbd>c</kbd>: _changes_ the selected text.
  Deletes the selected text and drops straight into insert mode.
  Shorthand for <kbd>di</kbd>.
  Like <kbd>d</kbd> if there's no selection it deletes the single character under the cursor (think of a cursor as a single-character selection).
* <kbd>Esc</kbd> to exit

Generally what you do in insert mode is just type characters into the buffer. But there are also some insert mode keyboard shortcuts ([docs](https://docs.helix-editor.com/keymap.html#insert-mode)):

* <kbd>Ctrl</kbd> + <kbd>w</kbd> or <kbd>Alt</kbd> + <kbd>Backspace</kbd>:
  deletes the word before the cursor.
  If the cursor is in the middle of a word deletes only the part of the word before the cursor.
  If the cursor is between words deletes the whitespace before the cursor plus the previous word.
* <kbd>Alt</kbd> + <kbd>d</kbd> or <kbd>Alt</kbd> + <kbd>Delete</kbd>:
  deletes the word _after_ the cursor. Similar behaviour if the cursor is between words or in the middle of a word.
* <kbd>Ctrl</kbd> + <kbd>u</kbd>:
  delete from cursor to start of line
* <kbd>Ctrl</kbd> + <kbd>k</kbd>:
  deletes from cursor to _end_ of line
* <kbd>Backspace</kbd> or <kbd>Ctrl</kbd> + <kbd>h</kbd> deletes the character before the cursor
* <kbd>Del</kbd> or <kbd>Ctrl</kbd> + <kbd>d</kbd> deletes the character _after_ the cursor
* <kbd>Enter</kbd> or <kbd>Ctrl</kbd> + <kbd>j</kbd> inserts a newline
* <kbd>Home</kbd> or <kbd>End</kbd>: go to start or end of line

### The dot command

"Repeat last insert".
This repeats both how you entered insert mode
(whether you used <kbd>i</kbd>, <kbd>a</kbd>, <kbd>I</kbd>, <kbd>A</kbd>, <kbd>o</kbd>, <kbd>O</kbd> or <kbd>c</kbd>) and then what you did in insert mode.

This is not as powerful as Vim's dot command: it's for inserts only.
If you want to do other things (e.g. deleting, or any other kind of change) repeatedly in Helix you either just manually repeat the change, or use multiple cursors/selections (tricky if not everything fits on one page), or use macros.

There's also an <kbd>Alt</kbd> + <kbd>.</kbd> command, which we saw earlier,
which repeats the last <kbd>f</kbd>, <kbd>t</kbd>, <kbd>F</kbd>, <kbd>T</kbd>, <kbd>[</kbd> or <kbd>]</kbd> motion.

### Undo

<kbd>u</kbd> undoes the most recent change,
<kbd>U</kbd> redoes the most recently undone change.

There's no support for persisting undo across sessions.

Each time you enter insert mode, make some changes, and exit insert mode, that's a single change as far as undo is concerned. You can do <kbd>Ctrl</kbd> + <kbd>s</kbd> in insert mode to place an undo checkpoint.

As well as undo and redo there are also `:earlier` (<kbd>Alt</kbd> + <kbd>u</kbd>) and `:later` (<kbd>Alt</kbd> + <kbd>U</kbd>) commands. These behave differently in how they traverse the undo tree: they move through all versions of the document chronologically, rather than moving through only some versions by going up one branch of the tree. They also accept a number of steps or a time span as argument, for example: `:earlier 3`, `:earlier 10s` or `:earlier '5 minutes'`.
This is apparently similar to `:earlier` and `:later` commands in Vim / Neovim, I didn't know these existed! Or I had forgotten. It's explained will in [this vimcasts screencast](http://vimcasts.org/episodes/undo-branching-and-gundo-vim/) (but note that Helix can't visualise the undo tree like vim's gundo plugin does).

(There are no command line versions of undo and redo, but you can access them via the command palette: <kbd>Space</kbd> <kbd>?</kbd>.)

### Copy-paste and registers

* <kbd>y</kbd> copies the selected text to the default register (<kbd>"</kbd>).
* <kbd>d</kbd> and <kbd>c</kbd> also copy the text that was deleted into the default register (<kbd>"</kbd>).
  You can use <kbd>Alt</kbd> + <kbd>d</kbd> or <kbd>Alt</kbd> + <kbd>c</kbd>
  to delete or change without copying into a register.
  As with <kbd>y</kbd> you can also delete into a named register with <kbd>"&lt;ch&gt;d</kbd> and <kbd>"&lt;ch&gt;c</kbd>.
* <kbd>Space</kbd> <kbd>y</kbd> copies the selected text to the system clipboard register (<kbd>+</kbd>).
  This is an alias for <kbd>"+y</kbd>.
  There's also a `:clipboard-yank` command for this.
* `:primary-clipboard-yank` copies the selected text into the system primary clipboard register (<kbd>*</kbd>).
  This is an alias for <kbd>"*y</kbd>.
* <kbd>p</kbd> pastes from the default register (<kbd>"</kbd>) after the cursor
* <kbd>P</kbd> pastes _before_ the cursor
* <kbd>Space</kbd> <kbd>p</kbd> and <kbd>Space</kbd> <kbd>P</kbd> paste from the system clipboard register (<kbd>+</kbd>) after and before the cursor. These are aliases for <kbd>"+p</kbd> and <kbd>"+P</kbd>.
  There are also `:clipboard-paste-after` and `:clipboard-paste-before` commands for this.
* `:primary-clipboard-paste-after` and `:primary-clipboard-paste-before`
* <kbd>R</kbd> replaces the current selection with the contents of the default register (<kbd>"</kbd>).
* <kbd>"&lt;ch&gt;R</kbd> replaces the current selection with the contents of register &lt;ch&gt;.
* <kbd>Space</kbd> <kbd>R</kbd> replaces the current selection with the system clipboard contents. This is an alias for <kbd>"+R</kbd>. There's also the `:clipboard-paste-replace` replace command for this.
* `:primary-clipboard-paste-replace` replaces the selected text with the contents of the system primary clipboard register (<kbd>*</kbd>).
  This is an alias for <kbd>"*R</kbd>.
* All paste commands accept a count, e.g. <kbd>100p</kbd> pastes the contents of the default register 100 times (yank commands don't accept a count).
* <code>:clear-register &lt;ch&gt;</code> to clear a register
  or `:clear-register` with no argument to clear all registers
* <kbd>Ctrl</kbd> + <kbd>r</kbd> <kbd>&lt;ch&gt;</kbd> pastes from register <code>&lt;ch&gt;</code> in insert mode, in prompts, in pickers.

Special registers:

* <kbd>/</kbd> for search
* <kbd>:</kbd> for last-executed command
* <kbd>"</kbd> for last-yanked text (this is the default register)
* <kbd>@</kbd> for last-recorded macro
* <kbd>_</kbd> is the black hole register
* <kbd>.</kbd> is contents of current selections
* <kbd>%</kbd> is name of current file
* <kbd>+</kbd> is system clipboard
* <kbd>*</kbd> is primary clipboard

### Match Mode

Match mode is for handling matching pairs of brackets, quotes, etc. It's inspired by vim-surround and vim-sandwich.

* Type <kbd>m</kbd> to enter **match mode**.
  You'll get a **Match** popup menu reminding you of the available key
  bindings in match mode.
* The supported surrounding pairs of characters are
  `{}`, `()`, `[]`, `<>`, `'` and `"`.
* When the cursor is over an opening or closing surrounding character <kbd>mm</kbd> jumps to the corresponding opening/closing character.
* When the cursor is inside a pair of surrounding characters then <kbd>mi&lt;ch&gt;</kbd> selects all the text within the pair of surrounding characters. For example with the cursor inside a pair of `(...)`'s either <kbd>mi(</kbd> or <kbd>mi)</kbd> will select the text inside the `(...)`'s.
* <kbd>mim</kbd> is a shortcut for selecting within the nearest pair of surrounding characters that Helix recognises.
* <kbd>ma&lt;ch&gt;</kbd> is the same as <kbd>mi&lt;ch&gt;</kbd> but also selects the surrounding characters themselves.
* <kbd>mam</kbd> selects the nearest surrounding pair of any type.
* Add surroundings to selected text: <kbd>ms&lt;ch&gt;</kbd>.
  This works with any <code>&lt;ch&gt;</code> but with `{}`, `()`, `[]` and `<>` it knows to use different opening and closing characters.
* Remove surroundings: <kbd>md&lt;ch&gt;</kbd> to delete surroundings.
* <kbd>mdm</kbd> removes the nearest pair of surrounding characters of any type.
* Replace surround: <kbd>mr&lt;ch&gt;&lt;ch&gt;</kbd>.
* <kbd>mrm&lt;ch&gt;</kbd> replaces the nearest pair of surrounding characters of any type.
* You can use counts to act on outer pairs.
  For example <kbd>2mi(</kbd> or <kbd>m2i(</kbd>.

You can also use <kbd>m</kbd> to do some other selections that aren't based on surrounding characters:

* <kbd>mig</kbd> and <kbd>miG</kbd> select changes (git)
* <kbd>miw</kbd>, <kbd>maw</kbd>, <kbd>miW</kbd> and <kbd>maW</kbd> select words.
* <kbd>mip</kbd> and <kbd>map</kbd> select paragraphs.
* <kbd>mif</kbd> selects the body of the current function,
  <kbd>maf</kbd> selects the body _and signature_.
* <kbd>mit</kbd> selects the body of the current type definition (i.e. class),
  <kbd>mat</kbd> selects the body and opening line.
* <kbd>mia</kbd> and <kbd>maa</kbd> select an argument/parameter in a function/method signature or call
* <kbd>mic</kbd> and <kbd>mac</kbd> select comments
* <kbd>miT</kbd> and <kbd>maT</kbd> select tests
* <kbd>mie</kbd> and <kbd>mae</kbd> select data structure entries

Multi-character surrounds (such as HTML tags, or ** and __ in Markdown) aren't supported, but the docs say this is planned in future.

### Search

* <kbd>/</kbd> opens the search prompt where you can enter a pattern to search for. See prompts above for keyboard shortcuts that're available in this prompt. Search uses regexes, for example `foo|bar|gar` to search for multiple things.
* <kbd>?</kbd> does a reverse-direction search.
* Once you've entered a search query you can jump between matches with <kbd>n</kbd> and <kbd>N</kbd>. Unlike in vim <kbd>n</kbd> always moves right and down and <kbd>N</kbd> always moves left and up, regardless of whether you did a forwards or a reverse search.
* All <kbd>/</kbd> and <kbd>?</kbd> actually do is enter text into the <kbd>/</kbd> register. You can also yank text into that register with <kbd>"/y</kbd>. You can paste the current search term into the document with <kbd>"/p</kbd> or <kbd>"/P</kbd> or into a prompt or picker or in insert mode with <kbd>Ctrl</kbd> + <kbd>r</kbd> <kbd>/</kbd>. And you can clear the register with `:clear-register /`.
* <kbd>\*</kbd> is an alias for <kbd>"/y</kbd>:
  it yanks the current selection into the search register.
  If the selection is a whole word this will also add regex word boundaries (`\b`) to the start and end of the search term. <kbd>Alt</kbd> + <kbd>*</kbd> does it without the word boundaries.

By default search uses smart-case and wrap-around, but both can be disabled inm your config file: https://docs.helix-editor.com/editor.html#editorsearch-section

There's no way to highlight all search matches at once.

There's a `make_search_word_bounded` (available via the command palette) to add `\b`'s to the current search term.

You can do something like a traditional "search and replace" like this:

* Enter a search term: <kbd>/</kbd> <kbd>foo</kbd> <kbd>Enter</kbd>.
  This will automatically jump to and select the first match.
* Replace the first match the same way you would replace any selection: <kbd>c</kbd> <kbd>bar</kbd> <kbd>Esc</kbd>.
* Go to the next match: <kbd>n</kbd> and replace it with <kbd>.</kbd>.
* Repeat <kbd>n</kbd> <kbd>.</kbd> for the rest of the matches.
* For any given match you can just skip over it by pressing <kbd>n</kbd> again without replacing, or you can delete it with <kbd>d</kbd> instead of replacing, or replace it with something else, etc.

### Global search

<kbd>Space</kbd> <kbd>/</kbd>. This is quite different from normal search. It uses a picker rather than the command line. It does however set the contents of the <kbd>/</kbd> register, so once you open a file from the picker you can already use <kbd>n</kbd> and <kbd>N</kbd> to move between the matches within that file.

The global search picker also suggests the contents of the <kbd>/</kbd> register (if any) when you first open it. Hit <kbd>Enter</kbd> to accept the suggestion. So to repeat the previous local or global search in the global search picker it's: <kbd>Space</kbd> <kbd>/</kbd> <kbd>Enter</kbd>.

Re-open global search picker with state preserved, as long as you haven't opened any other pickers in the meantime: <kbd>Space</kbd> <kbd>'</kbd>.

### Macros

Macros are a more powerful version of the <kbd>.</kbd> command discussed above (that repeats the last insertion). A macro lets you record any sequence of movements, edits and inserts, then move to another part of the file and repeat it.

* <kbd>Q</kbd> to start and stop recording a macro
* <kbd>q</kbd> to play back a macro
* Precede <kbd>Q</kbd> or <kbd>q</kbd> with <kbd>"&lt;ch&gt;</kbd> to record into a different macro register (it uses register <kbd>@</kbd> by default)

### Buffers

* Go to next or previous buffer: <kbd>gn</kbd>, <kbd>gp</kbd>,
  `:buffer-next`, `:bnext`, `:bn`,
  `:buffer-previous`, `:bprev`, `:bp`.
* Open a picker for all open buffers: <kbd>Space</kbd> <kbd>b</kbd>
* <kbd>ga</kbd> goes to the last-accessed file,
  <kbd>gm</kbd> goes to the last-modified file.
* Close the current buffer: `:buffer-close`, `:bclose`, `:bc`,
  `:buffer-close!`, `:bclose!`, `:bc!`.
* Save and close the current buffer:
  `:write-buffer-close`, `:wbc`,
  `:write-buffer-close!`, `:wbc!`
* Close _all_ buffers:
  `:buffer-close-all`, `:bcloseall`, `:bca`,
  `:buffer-close-all!`, `:bcloseall!`, `:bca!`
* Close all _other_ buffers except the current one:
  `:buffer-close-others`, `:bcloseother`, `:bco`,
  `:buffer-close-others!`, `:bcloseother!`, `:bco!`.
  Add the `--skip-visible` flag to _not_ close other buffers that're currently visible in any window split.

The `bufferline` settings lets you show a line across the top of the editor listing all open buffers. Set it to `always` to always show the bufferline, `multiple` to only show it when there are multiple buffers open, or `never` (the default) to always hide it:

    :set bufferline always

### Splits

Show multiple files (or the same file multiple times) at once in split windows.

* <kbd>Ctrl</kbd> + <kbd>w</kbd> or <kbd>Space</kbd> <kbd>w</kbd> opens the **Window menu** which has various keybindings for working with split windows.
* Opening new split windows:
  * <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>s</kbd>,
    <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>Ctrl</kbd> + <kbd>s</kbd>,
    or <kbd>Space</kbd> <kbd>w</kbd> <kbd>s</kbd>:
    open a new horizontal split window below the current one.
    The new split will be showing the same file as the previous split.
    The command `:hsplit`, `:hs` or `:sp` also does this.
    You can pass an argument to `:hsplit` / `:hs` / `:sp` to open an existing file (or create a new one if none with the given name exists) in a horizontal split. Tab-completion on the command line will work for file paths/names.
  * <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>v</kbd>,
    <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>Ctrl</kbd> + <kbd>v</kbd>,
    or <kbd>Space</kbd> <kbd>w</kbd> <kbd>v</kbd>:
    open a new _vertical_ split window to the right of the current one.
    The new split will be showing the same file as the previous split.
    The command `:vsplit` or `:vs` also does this.
    Again `:vsplit` / `:vs` can take an argument to create a new file or open an existing one.
  * <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>n</kbd> <kbd>s</kbd>,
    or <kbd>Space</kbd> <kbd>w</kbd> <kbd>n</kbd> <kbd>s</kbd>:
    open a new scratch buffer in a horizontal split window below the current one.
    The command `:hsplit-new` or `:hnew` also does this.
  * <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>n</kbd> <kbd>v</kbd>,
    or <kbd>Space</kbd> <kbd>w</kbd> <kbd>n</kbd> <kbd>v</kbd>:
    open a new scratch buffer in a _vertical_ split window to the right of the current one.
    The command `:vsplit-new` or `:vnew` also does this.
* Moving the cursor between split windows:
  * <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>w</kbd>,
    <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>Ctrl</kbd> + <kbd>w</kbd>,
    or <kbd>Space</kbd> <kbd>w</kbd> <kbd>w</kbd>:
    move the cursor to the "next" split window.
    This moves between split windows in the order in which they were opened.
    Once it gets to the newest split it cycles back to the oldest one.
  * <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>h</kbd>,
    <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>Ctrl</kbd> + <kbd>h</kbd>,
    or <kbd>Space</kbd> <kbd>w</kbd> <kbd>h</kbd>:
    move the cursor to split window left of the current one.
  * <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>j</kbd>,
    <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>Ctrl</kbd> + <kbd>j</kbd>,
    or <kbd>Space</kbd> <kbd>w</kbd> <kbd>j</kbd>:
    move the cursor to split window below the current one.
  * <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>k</kbd>,
    <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>Ctrl</kbd> + <kbd>k</kbd>,
    or <kbd>Space</kbd> <kbd>w</kbd> <kbd>k</kbd>:
    move the cursor to split window above the current one.
  * <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>l</kbd>,
    <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>Ctrl</kbd> + <kbd>l</kbd>,
    or <kbd>Space</kbd> <kbd>w</kbd> <kbd>l</kbd>:
    move the cursor to split window right of the current one.
* Moving split windows around:
  * <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>H</kbd>,
    <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>Ctrl</kbd> + <kbd>H</kbd>,
    or <kbd>Space</kbd> <kbd>w</kbd> <kbd>H</kbd>:
    swap the current split window with the one to its left.
  * <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>J</kbd>,
    <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>Ctrl</kbd> + <kbd>J</kbd>,
    or <kbd>Space</kbd> <kbd>w</kbd> <kbd>J</kbd>:
    swap the current split window with the one below it.
  * <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>K</kbd>,
    <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>Ctrl</kbd> + <kbd>K</kbd>,
    or <kbd>Space</kbd> <kbd>w</kbd> <kbd>K</kbd>:
    swap the current split window with the one above it.
  * <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>L</kbd>,
    <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>Ctrl</kbd> + <kbd>L</kbd>,
    or <kbd>Space</kbd> <kbd>w</kbd> <kbd>L</kbd>:
    swap the current split window with the one to its right.
  * <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>t</kbd>,
    <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>Ctrl</kbd> + <kbd>t</kbd>,
    or <kbd>Space</kbd> <kbd>w</kbd> <kbd>t</kbd>:
    "transpose" split windows.
    This seems to change horizontal splits into vertical splits and vice-versa.
* Closing split windows:
  * <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>q</kbd>,
    <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>Ctrl</kbd> + <kbd>q</kbd>,
    or <kbd>Space</kbd> <kbd>w</kbd> <kbd>q</kbd>:
    close the split window that the cursor is currently in.
  * <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>o</kbd>,
    <kbd>Ctrl</kbd> + <kbd>w</kbd> <kbd>Ctrl</kbd> + <kbd>o</kbd>,
    or <kbd>Space</kbd> <kbd>w</kbd> <kbd>o</kbd>:
    close all "other" split windows (all but the one the cursor is in).
* Opening other existing pickers in splits:
  * You can open horizontal and vertical splits from file-based packers (e.g. <kbd>Space</kbd> <kbd>f</kbd> or <kbd>Space</kbd> <kbd>b</kbd> etc) by selecting a file with <kbd>Ctrl</kbd> + <kbd>s</kbd> or <kbd>Ctrl</kbd> + <kbd>v</kbd> instead of <kbd>Enter</kbd>.
  * You can also open multiple files in splits from the command line when launching Helix with `--vsplit` and `--hsplit`.

You can't resize splits in Helix. Rather, it always keeps them all the same
size even as you resize Helix's terminal window. I'm not sure whether this is
by design or just because they haven't gotten around to it, but I think I quite
like it.

### Multiple cursors and selections

Very rough notes about this:

* <kbd>C</kbd> spawns a new cursor on the line below the lowest one.
* <kbd>Alt</kbd> + <kbd>C</kbd>
  (i.e. <kbd>Alt</kbd> + <kbd>Shift</kbd> + <kbd>c</kbd>)
  spawns a new cursor on the line _above_ the highest one.
* Both commands will skip over lines that aren't long enough.
* <kbd>,</kbd> collapses back down to just one cursor
* <kbd>s</kbd> prompts you for a regex and then spawns a separate cursor/selection for every match within the original selection
* <kbd>S</kbd> is sort of the reverse:
  it _splits_ the existing selection into separate selections,
  splitting on the regex pattern that you give.
* <kbd>Alt</kbd> + <kbd>s</kbd> splits selection on newlines,
  i.e. makes one selection per line of the original selection.
* <kbd>Alt</kbd> + <kbd>-</kbd> merges multiple selections back into one.
* <kbd>Alt</kbd> + <kbd>_</kbd> merges only consecutive selections
* `:yank-join` yanks multiple selections joined with newlines,
  accepts join char as arg
  Also `:clipboard-yank-join` and `:primary-clipboard-yank-join`
* <kbd>#</kbd> register is selection indices
* When there are multiple selections one selection is the primary one. It's highlighted differently from the others. <kbd>(</kbd> and <kbd>)</kbd> change which selection is the primary one.
  When you collapse back to one cursor/selection with <kbd>,</kbd> the primary selection is the one that gets kept.
* <kbd>Alt</kbd> + <kbd>,</kbd> removes the primary selection,
  keeping all others.
* <kbd>Alt</kbd> + <kbd>(</kbd> and <kbd>Alt</kbd> + <kbd>)</kbd> cycles the _contents_ of the selections.
* <kbd>&</kbd> aligns the contents of multiple selections.
* Another way to enter multiple selections is with the LSP select
references feature (<kbd>Space</kbd> <kbd>h</kbd>).
* In visual mode <kbd>n</kbd> and <kbd>N</kbd> add the next/prev match as a new selection rather than moving the primary selection. And you can toggle visual mode on and off so you can pick and choose which matches to add.
* `:sort` to sort the selections
* <kbd>_</kbd> trims leading and trailing whitespace from selection(s)  
* There are commands for yanking and pasting all or just the primary selection.
* <kbd>%</kbd> selects the whole file. There's a whole bunch of "selection manipulation" commands: https://docs.helix-editor.com/keymap.html#selection-manipulation

### Shell commands

There seems to be quite good shell integration which could be a good way to do lots of advanced things and to integrate external apps via keybindings.

Rough notes:

* <kbd>|</kbd> pipes the current selection(s) to a shell command and then replaces the selection(s) with the output from the shell command. Also `:|` or `:pipe` from the command line.
* <kbd>Alt</kbd> + <kbd>|</kbd> does the same but ignores the output of the shell command. This is `:pipe-to` on the command line.
* <kbd>!</kbd> runs a shell command and inserts its output before each selection. Also `:insert-output` from the command line.
* <kbd>Alt</kbd> + <kbd>!</kbd> _appends_ the output after each selection. Also `:append-output.`
* <kbd>$</kbd> pipes each selection into a shell command and keeps only those selections for which the shell command returned `0`.
* There's also `:!`, `:sh`, `:run-shell-command`. This runs a shell command and then displays its output in a popup window. This doesn't seem to have a default keybinding.
* And of course you can also suspend Helix and return to the shell that launched it with <kbd>Ctrl</kbd> + <kbd>z</kbd> and resume Helix again with `$ fg`.

Some possible example uses of shell commands: sorting with `sort`, filtering with `uniq`. Insert the current date from `date`. Opening the corresponding unit test file for a given script file. Global find-and-replace with sed or scooter.
 
### View mode (<kbd>z</kbd>) and sticky view (<kbd>Z</kbd>) mode

Sometimes you've done a bunch of work to position your cursor(s) and make selection(s) and you just want to scroll the view a little without changing your cursor positions/selections. Maybe something you want to see or select is just out of view. Or maybe you just want your cursor to be at the top or in the middle of the view. All the normal commands for scrolling up and down (<kbd>j</kbd>, <kbd>k</kbd>, <kbd>Page Down</kbd>, <kbd>Page Up</kbd>, etc) would mess up your carefully placed cursor(s)/selection(s).

**View mode** lets you scroll the view _without_ changing your cursor position(s)/selection(s). According to the docs it's: <q>intended for scrolling and manipulating the view without changing the selection</q>. You press <kbd>z</kbd> to enter view mode and you're presented with a popup menu of view mode key bindings:

* <kbd>z</kbd><kbd>z</kbd> or <kbd>z</kbd><kbd>c</kbd>:
  scroll up or down so that the primary selection is in the centre of the view.
  This scrolls the view so that the selection is centred, without changing the selection itself (rather than moving the selection to be in the centre of the view).
* <kbd>z</kbd><kbd>t</kbd>:
  scroll the view so that the primary selection is at the top.
  This won't necessarily place the primary selection's line at the very top of the view: it obeys the `scrolloff` setting which defaults to `5`, so by default <kbd>z</kbd><kbd>t</kbd> will scroll the view so that the primary selection's line is the fifth from the top.
  You can change this, for example: `:set scrolloff 0`.
* <kbd>z</kbd><kbd>b</kbd>:
  scroll the view so that the primary selection is at the _bottom_.
  As with <kbd>z</kbd><kbd>t</kbd>, this obeys `scrolloff`.
* <kbd>z</kbd><kbd>m</kbd>:
  scroll the view _horizontally_ so that the cursor is in the middle.
* <kbd>z</kbd><kbd>j</kbd>:
  scroll the view down by one line.
* <kbd>z</kbd><kbd>k</kbd>:
  scroll the view _up_ by one line.
* <kbd>z</kbd><kbd>Page Down</kbd> or <kbd>z</kbd><kbd>Ctrl</kbd> + <kbd>f</kbd>:
  scroll the view down by one page.
* <kbd>z</kbd><kbd>Page Up</kbd> or <kbd>z</kbd><kbd>Ctrl</kbd> + <kbd>b</kbd>:
  scroll the view _up_ by one page.
* <kbd>z</kbd><kbd>/</kbd> and <kbd>z</kbd><kbd>?</kbd>:
  search for a regex.
  Normally typing <kbd>/</kbd>, entering a regex, and hitting <kbd>Enter</kbd> would move the primary selection to the first regex match in the document. <kbd>z</kbd><kbd>/</kbd> keeps the selection(s) that you had and _adds a new selection for the first regex match_. So it enters multi-select mode. The new selection also becomes the primary selection.
  <kbd>n</kbd> and <kbd>N</kbd> will then add further selections for the next and previous regex matches, whereas <kbd>z</kbd><kbd>n</kbd> and <kbd>z</kbd><kbd>N</kbd> move the primary selection to the next and previous matches.

As with other minor modes, soon as you enter one of the above commands it automatically exits view mode. <kbd>Z</kbd> enters **sticky view mode** which has all the same keybindings as above but remains in view mode as you hit key bindings. Use <kbd>Esc</kbd> to exit.

[Key mapping docs](https://docs.helix-editor.com/keymap.html#view-mode).
There's also <kbd>z</kbd><kbd>Space</kbd> or <kbd>z</kbd><kbd>Ctrl</kbd> + <kbd>d</kbd> and <kbd>z</kbd><kbd>Backspace</kbd> or <kbd>z</kbd><kbd>Ctrl</kbd> + <kbd>u</kbd>. These move both the view and cursor down or up by half a page. As far as I can tell these don't even _pretend_ to be about scrolling the view without moving the cursor. They're no different from <kbd>Ctrl</kbd> + <kbd>d</kbd> and <kbd>Ctrl</kbd> + <kbd>u</kbd> in normal mode.

View mode doesn't quite work as advertised (<q>scroll without changing the selection</q>) because the primary selection always needs to be in view: if you use view mode to scroll the view enough so that the primary selection would be scrolled out of view, it absolutely does move the primary selection in order to keep it in view. This can easily be done with <kbd>z</kbd><kbd>Page Down</kbd> and <kbd>z</kbd><kbd>Page Up</kbd>, for example. This does seem to limit the usefulness of view mode quite a lot: if you just want to adjust the view by a little, but not so much that your primary selection would go out of view, you can use view mode to do that. Otherwise, I'm not sure what else view mode is useful for.

### Configuration

Helix has lots of config options ([docs](https://docs.helix-editor.com/editor.html)).

You can change a config option with the `:set-option` / `:set` command. For example to render all whitespace:

    :set whitespace.render all

Boolean options can also be toggled on and off with `:toggle-option` / `:toggle`. For example:

    :toggle soft-wrap.enable

You can also use `:get-option` / `:get` to print the current value of an option:

    :get search.wrap-around

To make config options permanent add them to a `config.toml` file: either `~/.config/helix/config.toml` for global config, or `.helix/config.toml` in the root of a workspace directory for per-workspace config. `:config-open` and `:config-open-workspace` are convenience commands for opening the `~/.config/helix/config.toml` and `.helix/config.toml` files. After editing a config file `:config-reload` will reload the config in your current Helix session so you don't have to quit and restart Helix.

Here's my current config:

```toml
# ~/.config/helix/config.toml

theme = "modus_operandi_deuteranopia"

[editor]
color-modes = true
popup-border = "popup"

[editor.auto-save]
focus-lost = true
after-delay.enable = true

[editor.cursor-shape]
insert = "bar"

[editor.search]
wrap-around = false

[editor.indent-guides]
render = true
skip-levels = 2

[editor.soft-wrap]
enable = true

[editor.whitespace.render]
space = "none"
tab = "all"
nbsp = "all"
nnbsp = "all"
newline = "all"
```

You can also use `config.toml` files to create custom keybindings: [docs](https://docs.helix-editor.com/remapping.html).

And you can put language-specific settings in `~/.config/helix/languages.toml` or `.helix/languages.toml` files: [docs](https://docs.helix-editor.com/languages.html).
