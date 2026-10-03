# Bash Calculator

A Bash script that takes two numbers as command-line arguments and prints their sum, difference, product and quotient. It stops with an error message if a zero is involved in the division.

## Usage

```bash
./calculator.sh <first number> <second number>
```

### Example

```
$ ./calculator.sh 10 5
Addition = 10 + 5 = 15
Subtraction = 10 - 5 = 5
Multiplication = 10 * 5 = 50
Division = 10 / 5 = 2
```

### Division by zero

```
$ ./calculator.sh 10 0
Addition = 10 + 0 = 10
Subtraction = 10 - 0 = 10
Multiplication = 10 * 0 = 0
Error: division reult is equal to 0
```

## How I built it, step by step

### 1. Create the file with Vim

```bash
vim calculator.sh
```

Press `i` to enter insert mode, then type the script (full script below).

### 2. Save and exit

Press `Esc`, then type:

```
:wq!
```

`:w` writes the file, `q` quits, and `!` forces it.

### 3. Make it executable

```bash
chmod +x calculator.sh
```

This gives the file permission to run as a program.

### 4. Run it

```bash
./calculator.sh 10 5
```

## The script, line by line

| Line | What it does |
|------|--------------|
| `#!/bin/bash` | The shebang. Tells the system to run the script with Bash. |
| `num1="$1"` | Stores the first argument in the variable `num1`. |
| `num2="$2"` | Stores the second argument in the variable `num2`. |
| `add=$((num1 + num2))` | Bash arithmetic with `$(( ))`. Stores the sum in `add`. |
| `min=$((num1 - num2))` | Stores the difference in `min`. |
| `mul=$((num1 * num2))` | Stores the product in `mul`. |
| `echo "Addition = ..."` | Prints the addition, subtraction and multiplication results. Double quotes let `$` variables expand. |
| `if [ $num1 -eq 0 ]` | Checks if the first number equals 0. If so, prints an error and exits with `exit 1`. |
| `elif [ $num2 -eq 0 ]` | Checks if the second number equals 0. If so, prints an error and exits with `exit 1`. |
| `div=$((num1 / num2))` | Stores the quotient in `div`. This line comes **after** the zero checks, so Bash never tries to divide by 0. |
| `echo "Division = ..."` | Prints the division result. |

## Bug I fixed

My first version had `div=$((num1 / num2))` near the top of the script, before the `if` check. Running `./calculator.sh 10 0` made Bash print its own error first:

```
./calculator.sh: line 10: num1 / num2: division by 0 (error token is "num2")
```

Moving the division line below the `if` / `elif` check means it only runs when `num2` is not 0, so only my own error message is shown.

## Full script

```bash
#!/bin/bash


num1="$1"
num2="$2"

add=$((num1 + num2))
min=$((num1 - num2))
mul=$((num1 * num2))


echo "Addition = $1 + $2 = $add"
echo "Subtraction = $1 - $2 = $min"
echo "Multiplication = $1 * $2 = $mul"

if
	[ $num1  -eq 0 ] ; then
	echo "Error: division reult is equal to 0"
	exit 1
elif
	[ $num2  -eq 0 ] ; then
	echo "Error: division reult is equal to 0"
	exit 1
fi

div=$((num1 / num2))
echo "Division = $1 / $2 = $div"
```

## Concepts practiced

Shebang, command-line arguments (`$1`, `$2`), variables, arithmetic expansion, `if` / `elif` conditions, exit codes, order of execution, `chmod`.
