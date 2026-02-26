# Vim Cheat Sheet — CKAD Essentials

Quick, minimal Vim commands you'll actually use during the CKAD exam.

## Why use Vim

- Often available in exam environments; fast once you know a few commands.
- If you're not comfortable, use a simpler editor (but practice a few Vim commands).

## Modes

- `i` : enter insert mode at cursor
- `Esc` : return to normal mode
- `v` : visual mode (select)
- `V` : visual-line mode
- `Ctrl-v` : visual-block mode

## Save & Exit

- `:w` : save
- `:q` : quit
- `:wq` or `:x` : save and quit
- `:q!` : quit without saving
- `ZZ` : save and quit (normal mode)

## Insert (quick)

- `i` : insert before cursor
- `I` : insert at line start
- `a` : append after cursor
- `A` : append at line end
- `o` / `O` : open new line below / above and enter insert

## Delete / Change

- `dd` : delete current line
- `D` : delete to end of line
- `x` : delete single character
- `cw` : change word (enters insert)

## Yank (copy) & Paste

- `yy` or `Y` : yank (copy) current line
- `yw` : yank word
- `p` : paste after cursor/line
- `P` : paste before cursor/line

## Undo / Redo

- `u` : undo
- `Ctrl-r` : redo

## Search & Replace

- `/pattern` then `Enter` : search forward
- `n` / `N` : next / previous match
- `:%s/old/new/g` : replace all in file
- `:%s/old/new/gc` : replace with confirmation

## Navigation

- `gg` : go to top of file
- `G` or `:n` : go to end / line n (e.g., `5G` or `:12`)
- `0` : start of line, `$` : end of line
- `w` / `b` : forward / backward by word

## Visual & Line Ops

- `v` then movement + `y` : visual select and yank
- `V` then movement + `>` / `<` : indent/unindent lines
- `:%y+` (if clipboard support) : yank whole file to system clipboard

## Useful Settings (temporary during exam)

- `:set number` : show line numbers
- `:set paste` then `i` (paste from external) ; `:set nopaste` afterwards

## Practical exam subset (keep memorized)

- Enter/exit insert: `i` / `Esc`
- Save/quit: `:w`, `:wq`, `:q!`
- Delete line: `dd`; yank line: `yy`; paste: `p`
- Undo/redo: `u` / `Ctrl-r`
- Search: `/pattern` + `n`
- Replace whole file: `:%s/old/new/gc`
- `:set paste` when pasting YAML from clipboard

---

Keep this file minimal and practice these commands a few times so they're muscle memory for the exam.
