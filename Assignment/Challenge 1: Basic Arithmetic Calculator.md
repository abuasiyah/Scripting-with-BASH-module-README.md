# Bash Calculator

A small Bash script that asks for two numbers and prints their sum, difference, product and quotient. It handles division by zero and rejects non-numeric input.

## Example

```
Enter first number: 10
Enter second number: 5
Results:
10 + 5 = 15
10 - 5 = 5
10 × 5 = 50
10 ÷ 5 = 2
```

Dividing by zero:

```
Enter first number: 10
Enter second number: 0
Results:
10 + 0 = 10
10 - 0 = 10
10 × 0 = 0
10 ÷ 0 = Error: cannot divide by zero
```

## How I built it, step by step

### 1. Create the file with Vim

```bash
vim calculator.sh
```

Press `i` to enter insert mode, then type the script (see the full script below).

### 2. Save and quit

Press `Esc`, then type:

```
:wq!
```

### 3. Make it executable

```bash
chmod +x calculator.sh
```

This gives the file permission to run as a program.

### 4. Run it

```bash
./calculator.sh
```

## How the script works

| Part | What it does |
|------|--------------|
| `#!/bin/bash` | The shebang. Tells the system to run the script with Bash. Must be the first line. |
| `read -p "Enter first number: " num1` | Prompts the user and stores what they type in `num1`. Same for `num2`. |
| `[[ "$num1" =~ ^-?[0-9]+$ ]]` | Regex check that the input is a whole number (negatives allowed). If not, the script prints an error and stops with `exit 1`. |
| `add=$((num1 + num2))` | `$(( ))` is Bash arithmetic. The result is stored in a variable. Same for subtraction and multiplication. |
| `echo "$num1 + $num2 = $add"` | Prints the result. Double quotes allow variables to be expanded. |
| `if [ "$num2" -eq 0 ]` | Checks for division by zero **before** dividing. Only the second number (the divisor) matters. |
| `exit 1` | Stops the script with a non-zero exit status, which signals an error. |
| `div=$((num1 / num2))` | Only reached when `num2` is not zero. |

## Full script

```bash
#!/bin/bash

# Prompt the user for two numbers
read -p "Enter first number: " num1
read -p "Enter second number: " num2

# Make sure both inputs are whole numbers
if ! [[ "$num1" =~ ^-?[0-9]+$ ]] || ! [[ "$num2" =~ ^-?[0-9]+$ ]]; then
  echo "Error: please enter whole numbers only."
  exit 1
fi

# Addition, subtraction, multiplication
add=$((num1 + num2))
sub=$((num1 - num2))
mul=$((num1 * num2))

echo "Results:"
echo "$num1 + $num2 = $add"
echo "$num1 - $num2 = $sub"
echo "$num1 × $num2 = $mul"

# Division: only the second number can't be zero
if [ "$num2" -eq 0 ]; then
  echo "$num1 ÷ $num2 = Error: cannot divide by zero"
  exit 1
fi

div=$((num1 / num2))
echo "$num1 ÷ $num2 = $div"
```

## Notes

- Bash arithmetic only handles **integers**, so `10 / 3` gives `3`, not `3.33`. For decimals you'd use a tool like `bc`.
- Concepts practiced: shebang, `read`, variables, arithmetic expansion, `if` conditions, exit codes, `chmod`.
