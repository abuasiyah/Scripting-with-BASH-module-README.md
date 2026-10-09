# DevOps Learning

My hands-on practice repo for learning DevOps. This section covers a set of **Bash scripting challenges**, built and tested on Ubuntu.

## Bash scripts

| # | Script | What it does |
|---|--------|--------------|
| 1 | `calculator.sh` | Takes two numbers as arguments and prints their sum, difference, product and quotient. Stops with an error instead of dividing by zero. |
| 2 | `file_operations.sh` | Creates a `bash_demo` directory, moves into it, writes a `demo.txt` file containing today's date, and prints the file's contents. |
| 3 | `file_checker.sh` | Asks for a filename, checks if it exists, then reports with ✓ or ✗ whether it is readable, writable and executable. |
| 4 | `backup.sh` | Asks for a source directory, creates a timestamped backup directory, copies all `.txt` files into it, and shows how many files were backed up. |

## Key learnings

1. **Order matters.** Bash runs top to bottom. In my calculator, the division ran *before* my zero check, so Bash threw its own error first. The check has to come before the risky command.
2. **Spaces and quotes are part of the syntax.** `[ ! -d "$dir" ]` needs spaces inside the brackets (`[!` gives `command not found`), and `my_var=value` must have no spaces around `=`. Quoting variables (`"$file"`) also protects against empty values and filenames with spaces.
3. **A variable name is not its value.** `read file` stores input, while `$file` reads it back. Writing `read $PATH` fails, and `read PATH` would overwrite the list of places Bash finds commands.
4. **Counters need `+ 1`.** `count=$((count ++))` never increased my counter, because `count ++` returns the old value. `count=$((count + 1))` works.
5. **Test the edge cases.** Missing files, empty folders, a zero divisor and typo'd paths revealed bugs that the happy path never did.

## A challenge I overcame

My backup script went wrong in three layers. First, `[: -d: integer expression expected` told me I had mixed a file test (`-d`) with a number comparison. After fixing that, `[!: command not found` showed I had missed a space after `[`. Once the script ran, it reported `Files backed up: 0` even though files were being copied, because of the `count ++` bug above. Later, a real run still printed `0` because I typed `home/sadak` instead of `/home/sadak`, a relative path instead of an absolute one.

Reading each error message carefully, reproducing it and fixing one thing at a time taught me more than the script itself did.

## Why Bash matters in DevOps

Working through these notes showed me that Bash is the layer that turns manual terminal work into repeatable automation. These are the parts that made it clear to me.

- **Repeatable automation.** A Bash script is a sequence of commands that runs automatically. With a shebang and `chmod +x`, a task I would type by hand becomes one command. With `$1` and `$2` parameters, one script can handle many inputs, like my calculator and my backup script.
- **My own toolkit, available anywhere.** Adding a scripts folder to `PATH` in `.bashrc` or `.zshrc` means my scripts run like any other command from any directory. That is how a personal library of DevOps tools gets built.
- **Scripts that adapt to the system.** Environment variables like `$HOME`, `$USER`, `$SHELL`, `$PWD` and `$OSTYPE` let one script work across different users and machines without hardcoded values.
- **Logic and repetition.** `if` / `elif` / `else`, `for` and `while` loops, `break` and `continue`, and arithmetic with `$(( ))` let scripts make decisions and handle many items, such as every `.txt` file in a backup.
- **Reusable, organised code.** Functions with parameters and `local` variables keep scripts modular, so I write logic once and call it many times.
- **Reliability and error handling.** Automation that fails silently is dangerous. Checking inputs first (like the division by zero example), using exit codes (`$?`, `exit 1`) and adding `set -euo pipefail` make scripts stop early with a clear message instead of carrying on in a broken state. `set -x` helps me trace what a script is really doing.
- **Safe handling of input.** Validating and sanitising user input (numbers only, no special characters) protects scripts from bad or unsafe data, which matters when scripts touch real systems.
- **Working with files and data.** Reading and writing files, piping commands together (`ls | wc -l`, `grep | awk`) and processing logs line by line is the daily work of troubleshooting and monitoring servers.
- **Integrity and security checks.** Generating and comparing checksums (`md5sum`, `sha256sum`) lets a script verify that a file has not been changed or corrupted, which is useful for downloads, backups and deployments.

Bash is available on most Linux servers, containers and CI runners, and tools like Docker and Git are driven from the command line, so these skills carry straight into the rest of the DevOps toolchain.
