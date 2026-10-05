# Challenge 3: File Checker with Permissions

A Bash script that checks if a file exists and shows whether it is readable, writable and executable.

## The challenge

Create a script that checks if a file exists and displays its permissions.

**Requirements**

- Prompt user for a filename
- Check if the file exists
- If it exists, check if it's readable, writable, and executable
- Display appropriate messages for each permission

**Example output**

```
Enter filename to check: /etc/passwd
File '/etc/passwd' exists.
✓ File is readable
✓ File is writable
✗ File is not executable
```

## Building the script, step by step

Each step below matches one requirement. The full script is at the bottom.

### Step 1: Shebang and function

```bash
#!/bin/bash

file_check() {
    ...
}

file_check
```

- `#!/bin/bash` tells the system to run the script with Bash. It must be the first line.
- `file_check() { ... }` defines a **function**, a named block of commands. All the steps below go between the curly braces.
- `file_check` on the last line **calls** the function. Defining a function does nothing until you call it.

### Step 2: Prompt the user for a filename

```bash
echo "Enter filename to check:"
read file
```

- `echo` prints the question.
- `read file` waits for the user to type something and stores it in the variable `file`.

### Step 3: Check if the file exists

```bash
if [[ -f $file ]]; then
    echo "File $file exists"
    ...
else
    echo "File '$file' does not exist."
fi
```

- `-f` is true if the path is a **regular file**. (Use `-e` instead if you want directories to count too.)
- If the file exists, the script runs the permission checks inside the `if`. If not, it prints a message and stops.
- `fi` is `if` backwards. It closes the `if` block.

### Step 4: Check the three permissions

Each check follows the same pattern, a yes/no test with a ✓ or ✗ message:

```bash
if [ -r "$file" ]; then
    echo "✓ File is readable"
else
    echo "✗ File is not readable"
fi
```

| Test | Meaning | Question it answers |
|------|---------|---------------------|
| `-r` | readable | Can I open it and read it? |
| `-w` | writable | Can I change it? |
| `-x` | executable | Can I run it as a program? |

The script uses the same pattern three times, once for `-r`, once for `-w` and once for `-x`. The quotes around `"$file"` keep filenames with spaces working.

## Create and run it with Vim

**1. Create the file**

```bash
vim file_checker.sh
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
chmod +x file_checker.sh
```

**5. Run it**

```bash
./file_checker.sh
```

## Full script

```bash
#!/bin/bash

file_check() {
    echo "Enter filename to check:"
    read file

    if [[ -f $file ]]; then
        echo "File $file exists"

        if [ -r "$file" ]; then
            echo "✓ File is readable"
        else
            echo "✗ File is not readable"
        fi

        if [ -w "$file" ]; then
            echo "✓ File is writable"
        else
            echo "✗ File is not writable"
        fi

        if [ -x "$file" ]; then
            echo "✓ File is executable"
        else
            echo "✗ File is not executable"
        fi
    else
        echo "File '$file' does not exist."
    fi
}

file_check
```

## Output

An existing file that is readable but not writable or executable:

```
Enter filename to check:
code.sh
File code.sh exists
✓ File is readable
✓ File is writable
✓ File is executable
```

A file that is also executable:

```
Enter filename to check:
math.sh
File math.sh exists
✓ File is readable
✓ File is writable
✓ File is executable
```

A file that does not exist:

```
Enter filename to check:
fake_file.sh
File 'fake_file.sh' does not exist.
```

## Notes

- The results depend on **who runs the script**. A regular user usually can't write to `/etc/passwd`, but `root` can, so root would see `✓ File is writable`.
- `-f` only matches regular files. A directory like `/etc` would get "does not exist". Change `-f` to `-e` to accept any type of file.

## Concepts practiced

Shebang, functions, `read`, `if` / `else` conditions, file test operators (`-f`, `-r`, `-w`, `-x`), `chmod`.
