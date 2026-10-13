# Linux CLI Editors
<div style="text-align: left;">
<img src="./images/cli editors.png" alt="Editor" width="600" >
</div>

## **ed** Editor
Old editor but avilable in all linux-unix systems, usefull if no option is avilable.
### File & Shell Operations

| Command | Description |
| --- | --- |
| `P` | Show the prompt (toggles the primary prompt)|
| `[addr[,addr]]cmd[params]` | Command Format (optional addresses, command, and parameters)|
| `!command` | Execute a shell command|
| `e filename` | Edit a file|
| `r filename` | Read file contents into buffer|
| `r !command` | Read command output into buffer|
| `w filename` | Write buffer to file|
|`2` `.` `$` `%` `+` `-` `,` `;` `/RE/`| Commands for location (of cursor)|
|`f` `p` `a` `c` `d` `i` `j` `s` `m` `u`|Editing commands|
| `q` | Quit|

### Location Commands

| Symbol | Description |
| --- | --- |
| `2` | A specific line number (in this example, line 2)|
| `.` | The current line address|
| `$` | The last line of the buffer|
| `%` | Shorthand for the entire file range (`1,$`)|
| `+` | Forward relative line offset (e.g., next line)|
| `-` | Backward relative line offset (e.g., previous line)|
| `,` | Separator for a line range (from first to second address)|
| `;` | Separator for a line range, setting current line (`.`) to the first address first|
| `/RE/` | Searches forward for a line matching the Regular Expression pattern (`RE`)|

---

### Editing Commands

| Command | Example | Description |
| --- | --- | --- |
| `f` |f document.txt | Show name of file being edited|
| `p` |1,5p | Print the current line|
| `a` |2a | Append at the current line|
| `c` |1,3c | Change the line|
| `d` |2d | Delete the current line|
| `i` |3i | Insert line at the current position|
| `j` |1,3j | Join lines|
| `s` |s/hello/ | Search for regex pattern|
| `m` |4,5m1 | Move current line to position|
| `u` |u | Undo latest change|
---
## **nano** Editor
Nano is a command-line text editor that comes pre-installed with most Linux distributions. It's designed to be user-friendly, with a simple interface that resembles popular graphical text editors. Nano provides essential editing features, making it ideal for quick edits, creating configuration files, or writing scripts directly in the terminal, blow is link for cheatsheet of nano.
### [nano cheatsheet](https://www.nano-editor.org/dist/latest/cheatsheet.html)
---

## **vi** Editor
The **vi** editor (Visual Editor) is a powerful, lightweight, terminal-based text editor pre-installed on almost every Linux and Unix-like operating system. Created by Bill Joy in 1976, it is highly resource-efficient and essential for managing servers or writing scripts where a graphical user interface (GUI) isn't available. On most modern Linux distributions, running vi opens **Vim** (Vi Improved), an upgraded version that supports enhanced features like syntax highlighting.

### [more on vim](https://vimhelp.org/)
- `vi`or`vim` Default in `command mode` while `ex/ed mode` commands are as above in `ed editor`.
<div style="text-align: left;">
<img src="./images/vi_mode.png" alt="Editor" width="600" >
</div>
<div style="text-align: left;">
<img src="./images/vi_ed1.png" alt="Editor" width="600" >
</div>
<div style="text-align: left;">
<img src="./images/vi_ed2.png" alt="Editor" width="600" >
</div>

- Also all those in **ed editor** like: `%s/search/replace/` (replace only first occurence in each line) OR `%s/search/replace/g` (globally all occurence in all lines)
<div style="text-align: left;">
<img src="./images/vi_ed3.png" alt="Editor" width="600" >
</div>
<div style="text-align: left;">
<img src="./images/vi_ed4.png" alt="Editor" width="600" >
</div>

## **emacs** Editor
It is not required to know both the editors, those who find `vi` hard can use `emacs` or other.
<div style="text-align: left;">
<img src="./images/emacs_1.png" alt="Editor" width="600" >
</div>
<div style="text-align: left;">
<img src="./images/emacs_2.png" alt="Editor" width="600" >
</div>