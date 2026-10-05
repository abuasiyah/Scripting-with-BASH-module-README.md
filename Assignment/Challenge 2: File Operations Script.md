# Challenge 2: File Operations Script

A Bash script that automates directory and file creation.

## The challenge

Create a script that automates directory and file creation.

**Requirements**

- Create a directory called `bash_demo`
- Navigate into the directory
- Create a file called `demo.txt`
- Write text to the file (include the current date)
- Display the file contents

**Example output**

```
Directory 'bash_demo' created.
File 'demo.txt' created.
File contents: This file was created by a Bash script on 2024-11-29
```

## Building the script, step by step

Each step below matches one requirement. The full script is at the bottom.

### Step 1: Shebang and variables

```bash
#!/bin/bash

dir_name="bash_demo"
file_name="demo.txt"
DATE=$(date +%F)
```

- `#!/bin/bash` tells the system to run the script with Bash. It must be the first line.
- `dir_name` and `file_name` store the names once, so they are easy to change later.
- `DATE=$(date +%F)` runs the `date` command and stores the result. `%F` formats it as `YYYY-MM-DD`.

### Step 2: Create the directory

```bash
mkdir "$dir_name"
echo "Directory '$dir_name' created."
```

- `mkdir` creates the directory. Bash replaces `$dir_name` with `bash_demo`.
- `echo` prints a message. Double quotes let variables expand, and the single quotes around the name are printed as text.

### Step 3: Navigate into the directory

```bash
cd "$dir_name"
echo "Current location: $PWD"
```

- `cd` moves into the new directory, so everything after this happens inside `bash_demo`.
- `$PWD` is a built-in variable holding the current directory. Printing it confirms the move.

### Step 4: Create the file and write text with the date

```bash
echo "This file was created by a $SHELL script on $DATE" > "$file_name"
echo "File '$file_name' created."
```

- `>` redirects the output of `echo` into the file instead of the screen. This creates `demo.txt` and writes the text.
- `$SHELL` is a built-in variable holding your shell (usually `/bin/bash`), and `$DATE` is the date from Step 1.
- A single `>` overwrites the file if it exists. `>>` would add to the end instead.

### Step 5: Display the file contents

```bash
echo "File contents: $(cat "$file_name")"
```

- `cat` outputs the text of the file.
- `$( )` captures that output so `echo` can print it after the label, on the same line.

## Create and run it with Vim

**1. Create the file**

```bash
vim file_operations.sh
```

**2. Enter insert mode and type the script**

Press `i`, then type or paste the full script below.

**3. Save and exit**

Press `Esc`, then type:

```
:wq
```

**4. Make it executable**

```bash
chmod +x file_operations.sh
```

**5. Run it**

```bash
./file_operations.sh
```

## Full script

```bash
#!/bin/bash

dir_name="bash_demo"
file_name="demo.txt"
DATE=$(date +%F)

mkdir "$dir_name"
echo "Directory '$dir_name' created."

cd "$dir_name"
echo "Current location: $PWD"

echo "This file was created by a $SHELL script on $DATE" > "$file_name"
echo "File '$file_name' created."

echo "File contents: $(cat "$file_name")"
```

## Output

```
Directory 'bash_demo' created.
Current location: /home/user/bash_demo
File 'demo.txt' created.
File contents: This file was created by a /bin/bash script on 2026-10-05
```

The location and date will match where and when you run it.

## Clean up

`mkdir` prints an error if `bash_demo` already exists, so delete the directory before running the script again:

```bash
cd ..
rm -r bash_demo
```

`rm -r` deletes permanently, so check the name before pressing Enter.

## Concepts practiced

Shebang, variables, command substitution `$( )`, `mkdir`, `cd`, `$PWD`, `$SHELL`, output redirection `>`, `cat`, `chmod`.
