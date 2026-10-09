# Challenge 4: Backup Script for Text Files

A Bash script that copies all `.txt` files from one directory into a new, timestamped backup directory and counts them.

## The challenge

Create a script that backs up all `.txt` files from one directory to another.

**Requirements**

- Prompt user for source directory
- Create a backup directory if it doesn't exist
- Copy all `.txt` files to the backup directory
- Add timestamp to backup directory name
- Display count of files backed up

**Example output**

```
Enter source directory: /home/user/documents
Backup directory created: backup_2024-11-29_14-30
Copying .txt files...
Backup complete! Files backed up: 5
```

## Building the script, step by step

Each step below matches one requirement. The full script is at the bottom.

### Step 1: Shebang and function

```bash
#!/bin/bash

Backup() {
    ...
}

Backup
```

- `#!/bin/bash` tells the system to run the script with Bash. It must be the first line.
- `Backup() { ... }` defines a **function**, a named block of commands. The steps below all go between the curly braces.
- `Backup` on the last line **calls** the function. Defining it does nothing until you call it.

### Step 2: Prompt for the source directory

```bash
echo "Enter source directory:"
read source_dir
```

- `echo` prints the question.
- `read source_dir` waits for the user to type and stores the answer in `source_dir`.

### Step 3: Add a timestamp to the backup directory name

```bash
timestamp=$(date +%Y-%m-%d_%H-%M)
backup_dir="$timestamp"
```

- `$(date +%Y-%m-%d_%H-%M)` runs the `date` command and captures the result, like `2026-10-09_06-39` (year-month-day_hour-minute).
- The result is stored in `timestamp`, then used as the backup directory name.

### Step 4: Create the backup directory if it doesn't exist

```bash
if [ ! -d "$backup_dir" ]; then
    mkdir "$backup_dir"
    echo "Backup directory created: $backup_dir"
fi
```

- `-d` tests "is this a directory?" and `!` means NOT, so the test reads "if the directory does **not** exist".
- Only then does it run `mkdir` and print the message.
- Note the space after `[` and before `]`. `[! -d ...` without the space gives `command not found`.

### Step 5: Copy the `.txt` files and count them

```bash
count=0
for file in "$source_dir"/*.txt; do
    if [ -f "$file" ]; then
        cp "$file" "$backup_dir"
        count=$((count + 1))
    fi
done
```

- `count=0` starts the counter at zero.
- `for file in "$source_dir"/*.txt` loops over every file ending in `.txt` in the source directory.
- `if [ -f "$file" ]` checks the file really exists. If the folder has no `.txt` files, Bash passes `*.txt` through as plain text, and this check stops the script from copying a file named `*.txt`.
- `cp "$file" "$backup_dir"` copies the file into the backup directory.
- `count=$((count + 1))` adds one to the counter for each file copied.

### Step 6: Display the count

```bash
echo "Backup complete! Files backed up: $count"
```

## Create and run it with Vim

**1. Create the file**

```bash
vim backup.sh
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
chmod +x backup.sh
```

**5. Run it**

```bash
./backup.sh
```

## Full script

```bash
#!/bin/bash

Backup() {
    echo "Enter source directory:"
    read source_dir

    timestamp=$(date +%Y-%m-%d_%H-%M)
    backup_dir="$timestamp"

    if [ ! -d "$backup_dir" ]; then
        mkdir "$backup_dir"
        echo "Backup directory created: $backup_dir"
    fi

    count=0
    for file in "$source_dir"/*.txt; do
        if [ -f "$file" ]; then
            cp "$file" "$backup_dir"
            count=$((count + 1))
        fi
    done

    echo "Backup complete! Files backed up: $count"
}

Backup
```

## Output

With a folder `documents` containing `notes.txt`, `todo.txt`, `ideas.txt` and `readme.md`:

```
Enter source directory:
./documents
Backup directory created: 2026-10-09_06-39
Backup complete! Files backed up: 3
```

The new `2026-10-09_06-39` folder contains `ideas.txt`, `notes.txt` and `todo.txt`. The `.md` file is not copied, and the original files stay where they were.

## Notes

- The backup folder is created in the directory you run the script from. The name depends on when you run it.
- The timestamp only goes down to the minute. Running the script twice in the same minute reuses the same folder, and files with the same name are overwritten.
- Only `.txt` files directly inside the source folder are copied, not those in subfolders.
- The script doesn't check that the source directory exists. A mistyped name gives `Files backed up: 0` instead of an error.
- To match the challenge's example folder name, use `backup_dir="backup_$timestamp"`.

## Bugs I fixed along the way

- `[! -d ...` needs a space after `[`. Without it Bash says `[!: command not found`.
- `count=$((count ++))` never increases the counter, because `count ++` returns the old value. Use `count=$((count + 1))`.

## Concepts practiced

Shebang, functions, `read`, command substitution `$( )`, `date` formatting, `if` conditions with `-d` and `-f`, `mkdir`, `for` loops with wildcards, `cp`, counters with `$(( ))`, `chmod`.
