# Bash Built-ins and Variables
> This document lists Bash built-in commands and variables.  Items are seperated by POSIX and Bash-maintained features.

> [!NOTE]
> Compatible with **Bash 5.2** and based on **POSIX.1-2017**.

**SECTIONS**: [Built-ins](#-built-ins) | [Variables](#-variables) | [Quick Reference](#-quick-reference) | [Sources](#-sources)

## 📁 Built-ins

### POSIX-Mandated Built-ins

(Required by the POSIX Shell standard — all POSIX-compliant shells provide these.)

| Built-in           | Purpose / Declaration                                                               |
| ------------------ | ----------------------------------------------------------------------------------- |
| `:`                | Null command; does nothing and returns success (`0`).                               |
| `.` *(dot)*        | Source and execute commands from a file in the current shell.                       |
| `alias`            | Define or display command aliases.                                                  |
| `bg`               | Resume a job in the background.                                                     |
| `break`            | Exit from a `for`, `while`, or `until` loop.                                        |
| `cd`               | Change the current working directory.                                               |
| `command`          | Execute a command, ignoring shell functions and aliases.                            |
| `continue`         | Resume the next iteration of a loop.                                                |
| `echo`             | Write text to standard output.                                                      |
| `eval`             | Evaluate arguments as a single command line.                                        |
| `exec`             | Replace the current shell with another program or modify redirections.              |
| `exit`             | Exit the current shell with an optional status code.                                |
| `export`           | Mark variables to be included in the environment of subsequently executed commands. |
| `false`            | Do nothing and return failure (`1`).                                                |
| `fg`               | Resume a job in the foreground.                                                     |
| `getopts`          | Parse positional parameters as options.                                             |
| `hash`             | Locate and remember command paths.                                                  |
| `jobs`             | Display status of jobs in the current shell.                                        |
| `kill`             | Send a signal to a process or job.                                                  |
| `pwd`              | Print the current working directory.                                                |
| `read`             | Read a line of input into variables.                                                |
| `readonly`         | Mark variables or functions as read-only.                                           |
| `return`           | Return from a shell function with a numeric status.                                 |
| `set`              | Set/unset shell options and positional parameters.                                  |
| `shift`            | Shift positional parameters to the left.                                            |
| `test` / `[` … `]` | Evaluate conditional expressions.                                                   |
| `times`            | Display accumulated user and system CPU time of the shell and its children.         |
| `trap`             | Specify commands to execute on receiving signals.                                   |
| `true`             | Do nothing and return success (`0`).                                                |
| `type`             | Display how a command name is interpreted by the shell.                             |
| `ulimit`           | Set/display resource limits of the shell and child processes.                       |
| `umask`            | Set or display the file mode creation mask.                                         |
| `unalias`          | Remove command aliases.                                                             |
| `unset`            | Remove variable or function definitions.                                            |
| `wait`             | Wait for background jobs or processes to terminate.                                 |

---

### Bash-Specific Built-ins

(Not required by POSIX — extensions provided by Bash.)

| Built-in                | Purpose / Declaration                                                                                |
| ----------------------- | ---------------------------------------------------------------------------------------------------- |
| `bind`                  | Set or display Readline key bindings.                                                                |
| `builtin`               | Execute a builtin command directly, bypassing functions or aliases.                                  |
| `caller`                | Display the current subroutine call stack.                                                           |
| `compgen`               | Generate completion matches for a word.                                                              |
| `complete`              | Specify how arguments for a command are auto-completed.                                              |
| `compopt`               | Modify options for programmable completion.                                                          |
| `declare` / `typeset`   | Declare variables with attributes (e.g., `-i` integer, `-a` array, `-A` associative, `-r` readonly). |
| `dirs`                  | Display the directory stack.                                                                         |
| `disown`                | Remove jobs from the shell’s job table so they won’t receive `SIGHUP`.                               |
| `enable`                | Enable or disable builtins.                                                                          |
| `fc`                    | Edit and re-execute commands from the history list.                                                  |
| `help`                  | Display help information about builtins.                                                             |
| `history`               | Display or manipulate the command history list.                                                      |
| `let`                   | Evaluate arithmetic expressions.                                                                     |
| `local`                 | Define variables local to a function.                                                                |
| `logout`                | Exit a login shell.                                                                                  |
| `mapfile` / `readarray` | Read lines from input into an array.                                                                 |
| `popd`                  | Remove the top directory from the directory stack.                                                   |
| `printf`                | Print formatted output (similar to C’s `printf`).                                                    |
| `pushd`                 | Add a directory to the directory stack and change to it.                                             |
| `shopt`                 | Enable or disable Bash-specific shell options.                                                       |
| `source`                | Synonym for `.` (dot); read and execute a file in the current shell.                                 |
| `suspend`               | Suspend the shell’s execution until resumed.                                                         |
| `time` *(keyword)*      | Measure and report the time for a pipeline’s execution.                                              |
| `caller`                | Report location in a script or function call stack.                                                  |

---

## 📁 Variables

### Exportable Environment Variables

(Propagate to child processes when exported. Many are standard in Unix-like environments.)

| Variable                   | Purpose / Declaration                                                |
| -------------------------- | -------------------------------------------------------------------- |
| `HOME`                     | Current user’s home directory.                                       |
| `PATH`                     | Colon-separated list of directories to search for commands.          |
| `USER` / `LOGNAME`         | Current login user name.                                             |
| `SHELL`                    | Path to the user’s login shell.                                      |
| `PWD`                      | Current working directory.                                           |
| `OLDPWD`                   | Previous working directory.                                          |
| `MAIL`                     | Path to the current user’s mailbox file.                             |
| `MAILCHECK`                | Interval in seconds between mail checks.                             |
| `IFS`                      | Internal Field Separator used for word splitting.                    |
| `LANG`, `LC_*`             | Locale and character set configuration.                              |
| `TERM`                     | Terminal type (e.g., `xterm-256color`).                              |
| `TMPDIR`                   | Default directory for temporary files.                               |
| `COLUMNS`                  | Terminal width in columns.                                           |
| `LINES`                    | Terminal height in rows.                                             |
| `PS1`, `PS2`, `PS3`, `PS4` | Prompt strings for primary, secondary, `select` loops, and tracing.  |
| `ENV` / `BASH_ENV`         | File executed at startup for non-interactive shells.                 |
| `CDPATH`                   | Search path for the `cd` builtin.                                    |
| `HISTFILE`                 | File where the command history is saved.                             |
| `HISTFILESIZE`             | Maximum number of lines in the history file.                         |
| `HISTSIZE`                 | Number of commands stored in the history list.                       |
| `HISTCONTROL`              | Controls how the history list handles duplicates and leading spaces. |

> [!NOTE]
> These become part of the **process environment** and are inherited by subshells or external programs once exported (`export VAR=value`).

---

### Bash-Maintained Special Variables

(Exist only inside Bash; they reflect shell state and are **not inherited** by child processes.)

| Variable                                                           | Purpose / Declaration                                                   |
| ------------------------------------------------------------------ | ----------------------------------------------------------------------- |
| `$#`                                                               | Number of positional parameters passed to the shell or function.        |
| `$*`                                                               | All positional parameters as a single word (joined by `IFS`).           |
| `$@`                                                               | All positional parameters as separate quoted words.                     |
| `$?`                                                               | Exit status of the last command.                                        |
| `$$`                                                               | PID of the current shell.                                               |
| `$!`                                                               | PID of the most recently executed background job.                       |
| `$-`                                                               | Current shell option flags.                                             |
| `$_`                                                               | Last argument of the previous command (or the command name in scripts). |
| `$0`                                                               | Name of the shell or the script being executed.                         |
| `$n`                                                               | Positional parameters (`$1`, `$2`, …).                                  |
| `PIPESTATUS`                                                       | Array with exit statuses of the most recent foreground pipeline.        |
| `BASH`                                                             | Full path to the Bash executable.                                       |
| `BASHOPTS`                                                         | Colon-separated list of enabled Bash options.                           |
| `BASHPID`                                                          | PID of the current Bash process.                                        |
| `BASH_ARGC`                                                        | Array with counts of positional parameters in each stack frame.         |
| `BASH_ARGV`                                                        | Array of all positional parameters in reverse order.                    |
| `BASH_COMMAND`                                                     | Command currently being executed.                                       |
| `BASH_ENV`                                                         | Startup file for non-interactive shells.                                |
| `BASH_EXECUTION_STRING`                                            | Command argument to `bash -c`.                                          |
| `BASH_LINENO`                                                      | Array of line numbers corresponding to each call in the stack.          |
| `BASH_REMATCH`                                                     | Array of regex matches from the last `[[ string =~ regex ]]` test.      |
| `BASH_SOURCE`                                                      | Array of source filenames corresponding to function definitions.        |
| `BASH_SUBSHELL`                                                    | Current subshell nesting level.                                         |
| `BASH_VERSINFO`                                                    | Array of Bash version components.                                       |
| `BASH_VERSION`                                                     | String with the current Bash version.                                   |
| `COMP_CWORD`, `COMP_LINE`, `COMP_POINT`, `COMP_WORDS`, `COMPREPLY` | Used by Bash programmable completion.                                   |
| `DIRSTACK`                                                         | Array representing the directory stack.                                 |
| `EUID`                                                             | Effective user ID of the shell.                                         |
| `FUNCNAME`                                                         | Array of function call stack.                                           |
| `GLOBIGNORE`                                                       | Colon-separated patterns to exclude from pathname expansion.            |
| `GROUPS`                                                           | Array of groups the current user belongs to.                            |
| `HISTCMD`                                                          | History number of the current command.                                  |
| `HOSTNAME`                                                         | Current host name.                                                      |
| `HOSTTYPE`                                                         | Hardware type Bash was built for.                                       |
| `LINENO`                                                           | Line number in the current script or shell.                             |
| `MAPFILE`                                                          | Default variable for `mapfile` if none given.                           |
| `OPTARG`                                                           | Argument to the last option processed by `getopts`.                     |
| `OPTIND`                                                           | Index of the next parameter for `getopts`.                              |
| `OSTYPE`                                                           | OS type string.                                                         |
| `PPID`                                                             | PID of the parent process of the shell.                                 |
| `PROMPT_COMMAND`                                                   | Commands executed before the primary prompt is displayed.               |
| `RANDOM`                                                           | Generates a pseudo-random integer each time it is referenced.           |
| `READLINE_LINE`, `READLINE_POINT`                                  | Used by Readline during editing.                                        |
| `REPLY`                                                            | Default variable for `read` when no variable is specified.              |
| `SECONDS`                                                          | Number of seconds since the shell started or since last reset.          |
| `SHELLOPTS`                                                        | Colon-separated list of currently enabled shell options.                |
| `SHLVL`                                                            | Current shell nesting level.                                            |
| `TIMEFORMAT`                                                       | Format string for output of the `time` keyword.                         |
| `UID`                                                              | Real user ID of the shell process.                                      |

---

## 📁 Quick Reference

* **POSIX Built-ins:** guaranteed in all compliant shells.
* **Bash-specific Built-ins:** available only in Bash.
* **Exportable Variables:** propagate to subshells and external programs once exported.
* **Bash-maintained Variables:** reflect shell internals; not inherited by child processes.

For details on your system:

```bash
help               # lists all built-ins
help <builtin>     # shows help for a specific builtin
man bash           # authoritative reference for builtins & special variables
```

---

## 📁 Sources

- [GNU Bash Reference Manual](https://www.gnu.org/software/bash/manual/)(current stable release) → Shell Builtin Commands and Bash Variables section.
- [POSIX.1-2017 (IEEE Std 1003.1-2017) specification for Shell Command Language](https://pubs.opengroup.org/onlinepubs/9699919799/) → Special Built-in Utilities section.
- `man bash` → Parameters, Shell Variables, and Special Parameters sections.
- GNU Bash manual → Shell Variables and Bash Conditional Expressions.
