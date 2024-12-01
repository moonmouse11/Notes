# Essentials
***
## General
Vim is a powerful and lightweight text editor that runs on virtually any platform and offers a rich set of features and customization options. However, Vim has a steep learning curve and can be intimidating for beginners or occasional users.

That’s where this Vim cheat sheet comes in handy. In this post, I’ve compiled a comprehensive and easy-to-follow guide to the most essential Vim commands and techniques, organized by category and explained with examples and tips.
- **`Esc`**/**`Ctrl+c`** : 📣command mode
- **`:q!`** : 🚪quit without saving
- **`:wq`**/**`:x`** : ✅🚪save and quit
- **`u`** : ↩️undo
- **`Ctrl + r`** : ↪️redo
- **`:set number`** : 🔢
---
## Insert
- **`i`** : ⤵️insert before cursor
- **`I`** : insert at the beginning of line
- **`a`** : insert after cursor
- **`o`** : insert new line
- **`:r file`** : insert external file at the location of the cursor
- **`:#r file`** : insert external file at line number #
---
## Movement
### Moving by Characters, Words and Tokens
- `h` – move the cursor left
- `j` – move the cursor down
- `k` – move the cursor up
- `l – move the cursor right
You can also use these keys with a number as a prefix to move in a specified direction multiple times. For example, if you run `5j` the cursor moves down 5 lines.
- `b` – move to the start of a word
- `B` – move to the start of a token
- `w` – move to the start of the next word
- `W` – move to the start of the next token
- `e` – move to the end of a word
- `E` – move to the end of a token
For instance, you have the noun phrase “step-by-step” as part of a text and the cursor is placed at the end of it. The first time you press `b`, the cursor moves back to “step-by-**s**tep”. However, if you use `B`, the cursor moves all the way back to: “**s**tep-by-step” since there is no whitespace between these characters.
### Moving by Lines
- `**0**` (zero) – jump to the beginning of the line
- `**$**` – jump to the end of the line
- `**^**` – jump to the first (non-blank) character of the line
- `**#G**` / `**#gg**` / `**:#**` – move to a specified line number (replace **#** with the line number)

# Moving by Screens

The following commands are used as a quick way to move within the text without scrolling.

- `**Ctrl + b**` – move back one full screen
- `**Ctrl + f**` – move forward one full screen
- `**Ctrl + d**` – move forward 1/2 a screen
- `**Ctrl + u**` – move back 1/2 a screen
- `**Ctrl + e**` – move screen down one line (without moving the cursor)
- `**Ctrl + y**` – move screen up one line (without moving the cursor)
- `**Ctrl + o**` – move backward through the jump history
- `**Ctrl + i**` – move forward through the jump history
- `**H**` – move to the top of the screen (H=high)
- `**M**` – move to the middle of the screen (M=middle)
- `**L**` – move to the bottom of the screen (L=low)


---
## Search Pattern
- **`/`** : ➡️search pattern (forward)
- **`?`** : ⬅️search pattern (backward)
- **`n`** : ➡️ / ⬅️find next (same direction)
- **`N`** : ➡️ / ⬅️find next (opposite direction)
- **`*`** : ➡️search current word (forward)
- **`#`** : ⬅️search current word (backward)
---
## Editing
### cut/copy/paste
- **`d`** : ❌✂️delete/cut selection
use with motions:
	- **`de`** : delete rest of the word
	- **`d)`** : delete rest of the sentence
	- **`dd`** : delete a line
	- **`d/?`** : delete until ?
- **`y`** : 📑copy
use with motions:
	- **`ye`** : copy rest of the word
	- **`y)`** : copy rest of the sentence
	- **`yy`** : copy line
- **`x`** : ❌✂️cut character/selection
- **`p`** : 📋paste (after the cursor)
### selection
- **`v`** : 🔖select mode
- **`V`** : select line
- **`Ctrl+v`** : select block
### replace
- **`r`** : ❌⤵️replace character
- **`s`** : delete character and insert
- **`c`** : ❌⤵️replace entire line or selection
use with motions:
	- **`ce`** : replace rest of the word
	- **`c)`** : replace rest of the sentence
	- **`cc`** : replace a line
	- **`c/?`** : delete until ? and replace with
- **`:%s/old/new/gc`** : 👴⤵️👦replace old pattern with new (global/confirm)

---
## Jumps, Marks, & Registers
### jump history
- **`Ctrl + i`** : ⏩ move curser forward in history
- **`Ctrl + o`** : ⏪ move curser backward in history
- **`:jumps`** : 📜see list of jumps
### Marks
- **`ma`** : ◎ mark current position as a
- **`'a`** : 🎯jump to mark a
- **`'.`** : ⏪jump to last edit
- **`:marks`** : 📜see list of marks
### register
- **`"ay`** : 📑copy to register a
- **`"ap`** : 📋paste from register a
- **`:reg`** : 📜see list of registers
---
## Multiple Files, Buffers, & Tabs
### split view
- **`:sp`** : 🏙🏙 split view (open new window/ same buffer)
	- or **`Ctrl+ws`**
- **`:vsp`** : split vertically
- **`Ctrl + ww`** : 🏙🔄🏙 switch between windows
- **`Ctrl + wc`** : 🏙close window
### buffers
- **`:e file`** : 🏙🌆 edit file in new buffer
	- **`:e!`** : reload file from disc
- **`Ctrl+g`** : ℹ️show file name and cursor location
- **`:ls`** : 📜see list of all buffers
- **`:bn`** : 🏙➡️🌆 move to next buffer
- **`:bp`** : 🏙⬅️🌆 move to previous buffer
- **`:b file`** : move to a specific buffer "file"
- **`:b #`** : move to buffer number #
### tabs
- **`:tabnew`** : 🗂open new tab
	- **`:tabnew file`** : open file in new tab
- **`:tabn`** : ▶️🗂next tab
- **`:tabp`** : 🗂◀️previous tab
***
Vim is a powerful and lightweight text editor that runs on virtually any platform and offers a rich set of features and customization options. However, Vim has a steep learning curve and can be intimidating for beginners or occasional users.

That’s where this Vim cheat sheet comes in handy. In this post, I’ve compiled a comprehensive and easy-to-follow guide to the most essential Vim commands and techniques, organized by category and explained with examples and tips.

# Moving by Characters, Words and Tokens

The basic keys for moving the cursor by one character are:

- `**h**` – move the cursor left
- `**j**` – move the cursor down
- `**k**` – move the cursor up
- `**l**` – move the cursor right

You can also use these keys with a number as a prefix to move in a specified direction multiple times. For example, if you run `**5j**` the cursor moves down 5 lines.

- `**b**` – move to the start of a word
- `**B**` – move to the start of a token
- `**w**` – move to the start of the next word
- `**W**` – move to the start of the next token
- `**e**` – move to the end of a word
- `**E**` – move to the end of a token

For instance, you have the noun phrase “step-by-step” as part of a text and the cursor is placed at the end of it. The first time you press `**b**`, the cursor moves back to “step-by-**s**tep”. However, if you use `**B**`, the cursor moves all the way back to: “**s**tep-by-step” since there is no whitespace between these characters.

# Moving by Lines

- `**0**` (zero) – jump to the beginning of the line
- `**$**` – jump to the end of the line
- `**^**` – jump to the first (non-blank) character of the line
- `**#G**` / `**#gg**` / `**:#**` – move to a specified line number (replace **#** with the line number)

# Moving by Screens

The following commands are used as a quick way to move within the text without scrolling.

- `**Ctrl + b**` – move back one full screen
- `**Ctrl + f**` – move forward one full screen
- `**Ctrl + d**` – move forward 1/2 a screen
- `**Ctrl + u**` – move back 1/2 a screen
- `**Ctrl + e**` – move screen down one line (without moving the cursor)
- `**Ctrl + y**` – move screen up one line (without moving the cursor)
- `**Ctrl + o**` – move backward through the jump history
- `**Ctrl + i**` – move forward through the jump history
- `**H**` – move to the top of the screen (H=high)
- `**M**` – move to the middle of the screen (M=middle)
- `**L**` – move to the bottom of the screen (L=low)

# Inserting Text

- `**i**` – switch to insert mode before the cursor
- `**I**` – insert text at the beginning of the line
- `**a**` – switch to insert mode after the cursor
- `**A**` – insert text at the end of the line
- `**o**` – open a new line below the current one
- `**O**` – open a new line above the current one
- `**ea**` – insert text at the end of the word
- `**Esc**` – exit insert mode; switch to command mode

Some of these commands switch between **command** and **insert mode**. By default, Vim launches in command mode, allowing you to move around and edit the file. To switch to command mode, use the **Esc** key.

On the other hand, the insert mode enables you to type and add text into the file. To move to insert mode, press `**i**`.

# Editing Text

- `**r**` – replace a single character (and return to command mode)
- `**cc**` – replace an entire line (deletes the line and moves into insert mode)
- `**C**` / `**c$**` – replace from the cursor to the end of a line
- `**cw**` – replace from the cursor to the end of a word
- `**s**` – delete a character (and move into insert mode)
- `**J**` – merge the line below to the current one with a space in between them
- `**gJ**` – merge the line below to the current one with no space in between them
- `**u**` – undo
- `**Ctrl**` + `**r**` – redo
- `**.**` – repeat last command

# Cutting, Copying And Pasting

- `**yy**` – copy (yank) entire line
- `**#yy**` – copy the specified number of lines
- `**dd**` – cut (delete) entire line
- `**#dd**` – cut the specified number of lines
- `**p**` – paste after the cursor
- `**P**` – paste before the cursor

# Marking Text (Visual Mode)

Apart from command mode and insert mode, Vim also includes **visual mode**. This mode is mainly used for marking text.

Based on the chunk of text you want to select, you can choose between three versions of visual mode: **character mode**, **line mode**, and **block mode**.

- `**v**` – select text using character mode
- `**V**` – select lines using line mode
- `**Ctrl**`+`**v**` – select text using block mode

Once you have enabled one of the modes, use the navigation keys to select the desired text.

- `**o**` – move from one end of the selected text to the other
- `**aw**` – select a word
- `**ab**` – select a block with ()
- `**aB**` – select a block with {}
- `**at**` – select a block with <>
- `**ib**` – select inner block with ()
- `**iB**` – select inner block with {}
- `**it**` – select inner block with <>

# Visual Commands

Once you have selected the desired text in visual mode, you can use one of the visual commands to manipulate it. Some of them include:

- `**y**` – yank (copy) the marked text
- `**d**` – delete (cut) the marked text
- `**p**` – paste the text after the cursor
- `**u**` – change the market text to lowercase
- `**U**` – change the market text to uppercase

# Search in File

- `*****` – jump to the next instance of the current word
- `**#**` – jump to previous instance of the current word
- `**/pattern**` – search forward for the specified pattern
- `**?pattern**` – search backward for the specified pattern
- `**n**` – repeat the search in the same direction
- `**N**` – repeat the search in the opposite direction

# Saving and Exiting File

- `**:w**` – save the file
- `**:wq**` / `**:x**` / `**ZZ**` – save and close the file
- `**:q**` – quit
- `**:q!**`/ `**ZQ**` – quit without saving changes
- `**:w new_file_name**` – save the file under a new name and continue editing the original
- `**:sav**` – save the file under a new name and continue editing the new copy
- `**:w !sudo tee %**` – write out the file using sudo and s[ee command](https://phoenixnap.com/kb/linux-tee)

# Working with Multiple Files

- `**:e file_name**` – open a file in a new buffer
- `**:bn**` – move to the next buffer
- `**:bp**` – go back to previous buffer
- `**:bd**` – close buffer
- `**:b#**` – move to the specified buffer (by number)
- `**:b file_name**` – move to a buffer (by name)
- `**:ls**` – list all open buffers
- `**:sp file_name**` – open a file in a new buffer and split [viewport](https://phoenixnap.com/glossary/viewport-definition) horizontally
- `**:vs file_name**` – open a file in a new buffer and split viewport vertically
- `**:vert ba**` – edit all files as vertical viewports
- `**:tab ba**` – edit all buffers as tabs
- `**gt**` – move to next tab
- `**gT**` – move to previous tab
- `**Ctrl+ws**` – split viewport
- `**Ctrl+wv**` – split viewport vertically
- `**Ctrl+ww**` – switch viewports
- `**Ctrl+wq**` – quit a viewport
- `**Ctrl+=**` – make all viewports equal in height and width

# Marks and Jumps

- `**m[a-z]**` – mark text using character mode (from `**a**` to `**z**`)
- `**M[a-z]**` – mark lines using line mode (from `**a**` to `**z**`)
- ``**`a**`` - jump to position marked `**a**`
- ``**`y`a**`` – yank text to position marked **>**`**a>**`
- ``**`.**`` – jump to last change in file
- ``**`0**`` – jump to position where Vim was last exited
- `**``**` – jump to last jump
- `**:marks**` – list all marks
- `**:jumps**` – list all jumps
- `**:changes**` – list all changes
- `**Ctrl+i**` – move to next instance in jump list
- `**Ctrl+o**` – move to previous instance in jump list
- `**g,**` – move to next instance in change list
- `**g;**` – move to previous instance in change list

# Macros

- `**qa**` – record macro `**a**`
- `**q**` – stop recording macro
- `**@a**` – run macro `**a**`
- `**@@**` – run last macro again