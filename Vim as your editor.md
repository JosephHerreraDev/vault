---
date: 2025-09-03
tags:
  - resource/video/tutorial/tools/vim
source: https://www.youtube.com/playlist?list=PLm323Lc7iSW_wuxqmKx_xxNtJC_hJbQ7R
completed: complete
---
# General description

The Primeagen course on Vim.

---
# Notes

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