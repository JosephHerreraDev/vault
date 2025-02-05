---
date: 2025-02-04
tags:
  - notes
---
# Vimtutor

## Cheat Sheet

| **Mode** | **Key**         | **Action**                      |
| -------- | --------------- | ------------------------------- |
| Normal   | `i`             | Enter insert mode               |
| Normal   | `Esc`           | Exit insert mode to normal mode |
| Normal   | `:w`            | Save file                       |
| Normal   | `:q`            | Quit Vim                        |
| Normal   | `:wq`           | Save and quit                   |
| Normal   | `:q!`           | Quit without saving             |
| Normal   | `:e <filename>` | Open file                       |
| Normal   | `:r <filename>` | Insert content from file        |
| Normal   | `u`             | Undo last action                |
| Normal   | `Ctrl + r`      | Redo undone action              |
| Normal   | `x`             | Delete character under cursor   |
| Normal   | `dd`            | Delete current line             |
| Normal   | `yy`            | Copy (yank) current line        |
| Normal   | `p`             | Paste after cursor              |
| Normal   | `P`             | Paste before cursor             |

---

### **Navigation in Normal Mode**

| **Key**    | **Action**                              |
| ---------- | --------------------------------------- |
| `h`        | Move left                               |
| `j`        | Move down                               |
| `k`        | Move up                                 |
| `l`        | Move right                              |
| `w`        | Jump to the start of the next word      |
| `b`        | Jump to the start of the previous word  |
| `0`        | Move to the beginning of the line       |
| `$`        | Move to the end of the line             |
| `gg`       | Move to the beginning of the file       |
| `G`        | Move to the end of the file             |
| `Ctrl + u` | Scroll half-page up                     |
| `Ctrl + d` | Scroll half-page down                   |
| `%`        | Jump to matching parenthesis or bracket |

---

### **Text Editing**

| **Mode** | **Key** | **Action**                                        |
| -------- | ------- | ------------------------------------------------- |
| Normal   | `cw`    | Change word (deletes word and enters insert mode) |
| Normal   | `C`     | Change from cursor to end of line                 |
| Normal   | `r`     | Replace a single character                        |
| Normal   | `~`     | Toggle case of character under cursor             |
| Visual   | `v`     | Start visual mode                                 |
| Visual   | `V`     | Start visual line mode                            |
| Visual   | `d`     | Delete selected text                              |
| Visual   | `y`     | Copy (yank) selected text                         |
| Visual   | `>`     | Indent selected text                              |
| Visual   | `<`     | Unindent selected text                            |

---

### **Search and Replace**

| **Mode** | **Key**         | **Action**                                              |
| -------- | --------------- | ------------------------------------------------------- |
| Normal   | `/text`         | Search for "text"                                       |
| Normal   | `?text`         | Search backwards for "text"                             |
| Normal   | `n`             | Go to next search result                                |
| Normal   | `N`             | Go to previous search result                            |
| Normal   | `:%s/old/new/g` | Replace all occurrences of "old" with "new" in the file |
| Normal   | `:noh`          | Clear search highlighting                               |

---

### **Working with Multiple Files**

| **Mode** | **Key**              | **Action**                        |
| -------- | -------------------- | --------------------------------- |
| Normal   | `:tabnew <filename>` | Open a file in a new tab          |
| Normal   | `gt`                 | Go to the next tab                |
| Normal   | `gT`                 | Go to the previous tab            |
| Normal   | `:sp <filename>`     | Open a file in a horizontal split |
| Normal   | `:vsp <filename>`    | Open a file in a vertical split   |
| Normal   | `Ctrl + w, w`        | Switch between splits             |
| Normal   | `Ctrl + w, q`        | Close current split               |

## Lesson 1
1. The cursor is moved using either the arrow keys or the `hjkl` keys.
	h(left) j(down) k(up) l(right)
2. To start Vim from the shell prompt type: `vim FILENAME <ENTER>`
3. To exit Vim type: `<ESC> :q! <ENTER>` to trash all changes.
   OR type: `<ESC> :wq <ENTER>` to save the changes.
4. To delete the character at the cursor type: x
5. To insert or append text type:
	i type inserted text `<ESC>` insert before the cursor
	A type inserted text `<ESC>` append after the line

Note: Pressing `<ESC>` will replace you in Normal mode or will cancel an unwanted and partially completed command.

## Lesson 2
1. To delete from the cursor up to the next word type: `dw`
2. To delete from the cursor up to the end of the word type: `de`
3. To delete from the cursor up to the end of a line type: `d$`
4. To delete a whole line type: `dd`
5. To repeat a motion prepend it with a number: `2w`
6. To format for a change command is:
`operator [number] motion`
where:
	operator - is what to do, such as d for delete
	[number] - is an optional count to repeat the motion
	motion - moves over the text to operate on, such as w (word), e (end of a word), $(end of a line), etc.
7. To move to the start of the line use a zero: `0`
8. To undo previous actions, type: `u`
   To undo all the changes on a line, type: `U` 
   To undo the undo's, type: `CTRL-R`
## Lesson 3
1. To put back text that has just been deleted, type `p` . This puts the deleted text AFTER the cursor (if a line was deleted it will go on the line below the cursor).
2. To replace the character under the cursor, type `r` and then the character you want to have there.
3. To change operator allows you to change from the cursor to where the motion takes you. eg. Type `ce` to change from the cursor to the end of the word, `c$` to change the end of a line.
4. The format for change is:
	`c [number] motion`
## Lesson 4
1. `CTRL-G` displays your location in the file and the file status.
   `G` moves to the end of the file.
   `number G` moves to that line number.
   `gg` moves to the first line.
2. Typing / followed by a phrase searches FORWARD for the phrase.
3. Typing ? followed by a phrase searches BACKWARD for the phrase.
	after a search type n to find the next occurrence in the same direction or N to search in the opposite direction.
	`CTRL-O` takes you back to older positions, `CTRL-I` to newer positions.
4. To substitute new for the first old in a line type `:s/old/new`
   To substitute new for all 'old's on a line type `:s/old/new/g`
   To substitute phrases between two line #'s type `:#,#s/old/new/g`
   To substitute all occurrences in the file type `:%s/old/new/g`
   To ask for confirmation each time add 'c'  `:%s/old/new/g`
## Lesson 5
1. `:!command` executes an external command.
some useful examples are:
`:!ls` - shows a directory listing.
`:!rm FILENAME` - removes file FILENAME.
2. `:w FILENAME` writes the current Vim file to disk with name FILENAME.
3. `v motion :w FILENAME` saves the Visually selected lines in file FILENAME.
4. `:r FILENAME` retrieves disk file FILENAME and puts it below the cursor position.
5. `:r !dir` reads the output of the dir command and puts it below the cursor position
## Lesson 6
1. Type `o` to open a line BELOW the cursor and start Insert mode.
   Type `O` to open a line ABOVE the cursor.
2. Type `a` to insert text AFTER the cursor.
   Type `A` to insert text after the end of the line.
3. The `e` command moves to the end of a word.
4. The `y` operator yanks(copies) text, `p` puts (pastes) it.
5. The `shift + y` operator yanks the whole line.
6. Typing a capital `R` enters Replace mode until `<ESC>` is pressed.
7. Typing "`:set xxx`" sets the options "xxx". some options are:
   `'ic' 'ignorecase'` ignore upper/lower case when searching.
   `'is' 'incsearch'` show partial matches for a search phrase.
   `'hls' 'hlsearch'` highlight all matching phrases
   You can either use the long or the short option name.
7. Prepend "no" to switch an option off: `:set noic`
## Lesson 7
1. Type `:help` or press `<F1>` or  `<HELP>` to open a help window.
2. Type `:help cmd` to find help on cmd.
3. Type `CTRL-W` `CTRL-W` to jump to another window.
4. Type `:q` to close the help window.
5. Create a `vimrc` startup script to keep your preferred settings.
6. When typing a : command, press `CTRL-D` to see possible completions. Press `<TAB>` to use one completion.

# Vim as your editor
[video series](https://www.youtube.com/playlist?list=PLm323Lc7iSW_wuxqmKx_xxNtJC_hJbQ7R)
## Introduction

> Don't grind memorizing.

**Modal editor**
1. Normal mode
2. Insert mode
3. Visual mode: `shift` + `v` select whole line
4. Command mode
## Horizontal

**Motions**
`_` beginning of a line
`$` end of a line
`0` beginning character
`f` + `character` moves into a character up front
`F` + `character` moves into a character to the back 
`t` + `character` moves up to a character up front
`T` + `character` moves up to a character to the back 
`,` backwards
`;` forward
`I` Beginning of a line in insert mode
`A` End of a line in insert mode
`o` Make a new line in insert mode
`O` Make a new line above in insert mode
## Vertical movements

> Not recommended	

`{` up by paragraph
`}` down by paragraph

`ctrl` + `d` jump half page down
`ctrl` + `u` jump half page up
`zz` center view
`gg` go to top
`G` go to bottom
`:` + `line number` go to specific line
`/` + `character` + `enter` search for character
`n` go to next character
`N` go to previous character
`?` + `character` + `enter` search for character backwards
`*` on a work, will search for that word
`#` on a work, will search for that word backwards

## Advanced horizontal motions
`v` + `i` + `(, {` select everything between parentheses (even if not inside the parentheses)
`v` + `a` + `(, {` select everything between parentheses including parentheses
`y` + `a` + `(, {` yank everything between parentheses
`v` + `i` + `w` selects a word 
`v` + `i` + `W` selects all text until white space

## Advanced vertical motions
`d` + `i` `(, {` delete text between brackets
`d` + `a` `(, {` delete text between brackets including the brackets

` = ` auto indents
` = + a + p` indents whole paragraph 
`>` + `number` + `motion` indent a number of lines
`V` turn a selection into line mode
`o` in visual mode, move to beginning and end of selection
`y` + `a` + `p` select continuous text 
`d` + `a` + `p` delete continuous text 
`g` + `ctrl` + `a` increment a selected group
`ctrl` + `a` increment

# Other resources
[Vim Tips You Probably Never Heard of](https://www.youtube.com/watch?v=bQfFvExpZDU)
[How to Do 90% of What Plugins Do (With Just Vim)](https://www.youtube.com/watch?v=XA2WjJbmmoM)

