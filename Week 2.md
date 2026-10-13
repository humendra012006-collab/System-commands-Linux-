# Linux CLI Editors
![Editors](./images/cli%20editors.png)

<div style="text-align: center;">
<img src="./images/cli editors.png" alt="Editor" width="300" height="200">
</div>

## **ed** Editor
Old editor but avilable in all linux-unix systems, usefull if no option is avilable

### File & Shell Operations

| Command | Description |
| --- | --- |
| `P` | Show the prompt (toggles the primary prompt)|
| `[addr[,addr]]cmd[params]` | Command Format (optional addresses, command, and parameters)|
| `!command` | Execute a shell command|
| `e filename` | Edit a file|
| `r filename` | Read file contents into buffer|
| `r !command` | Read command output into buffer|
| `w filename` | Write buffer to filename|
|`2` `.` `$` `%` `+` `-` `,` `;` `/RE/`| Commands for location|
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

| Command | Description |
| --- | --- |
| `f` | Show name of file being edited|
| `p` | Print the current line|
| `a` | Append at the current line|
| `c` | Change the line|
| `d` | Delete the current line|
| `i` | Insert line at the current position|
| `j` | Join lines|
| `s` | Search for regex pattern|
| `m` | Move current line to position|
| `u` | Undo latest change|
---

