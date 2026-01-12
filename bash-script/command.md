# Bash Script

### 🧩 Example: Simple Bash Script
`script.sh`
```bash
#!/bin/bash
echo "Hello, Sourov!"
```
`#!/bin/bash` `#!  → shebang` এর কাজ হলো এই স্ক্রিপ্টটা কোন প্রোগ্রাম দিয়ে চালাতে হবে সেটা অপারেটিং সিস্টেমকে জানানো।

### 🧩 Example: Using Variables
`script.sh`
```bash
name="Sourov"
echo "Hello, $name!"
```

### 🧩 Example: Numbers
`script.sh`
```bash
#!/bin/bash
num1=5
num2=10
sum=$((num1 + num2))
difference=$((num2 - num1))
product=$((num1 * num2))
quotient=$((num2 / num1))
echo "Sum: $sum, Difference: $difference, Product: $product, Quotient: $quotient"
```

### 🧩 Example: For & While Loop

```bash
count=1
while [ $count -le 5 ]; do
  echo "Count is $count"
  ((count++))
done

# Until loop example
count=1
until [ $count -gt 5 ]; do
  echo "Count is $count"
  ((count++))
done

# Break and continue example
for i in {1..5}; do
  if [ $i -eq 3 ]; then
    continue
  fi
  echo "Number $i"
  if [ $i -eq 4 ]; then
    break
  fi
done

# Nested loops example
for i in {1..3}; do
  for j in {1..2}; do
    echo "Outer loop $i, Inner loop $j"
  done
done

```
### 🧩 Example: Function

```bash
showName() {
  local name=$1
  echo "Hello, $name!"
}
showName "Alice"

add() {
  local sum=$(($1 + $2))
  echo $sum
}
result=$(add 5 3)
echo "Sum is $result"


```

# 🧩 Bash Operators

### 🧩 Comparison Operators
```bash
#!/bin/bash

a=10
b=20

if [ $a -lt $b ]; then
    echo "$a is less than $b"
else
    echo "$a is NOT less than $b"
fi
```
| Operator | Meaning |
|----------|---------|
| -eq | Equal to |
| -ne | Not equal to |
| -lt | Less than |
| -le | Less than or equal to |
| -gt | Greater than |
| -ge | Greater than or equal to |

### 🧩 String Comparison Operators

```bash
#!/bin/bash

str1="apple"
str2="banana"

if [ "$str1" = "$str2" ]; then
    echo "Strings are equal"
else
    echo "Strings are NOT equal"
fi
```

| Operator | Meaning |
|----------|---------|
| =  | Equal to |
| != | Not equal to |
| <  | Less than (ASCII alphabetical order) |
| >  | Greater than (ASCII alphabetical order) |


### 🧩 Arithmetic Operators

```bash
echo "2^3" | bc
# Bash নিজে ^ (power) সাপোর্ট করে না
# তাই bc বা awk ব্যবহার করতে হয়
```

| Operator | Meaning | Example | Result |
|----------|---------|---------|--------|
| + | Addition | `echo $((5 + 3))` | 8 |
| - | Subtraction | `echo $((5 - 3))` | 2 |
| * | Multiplication | `echo $((5 * 3))` | 15 |
| / | Division | `echo $((10 / 2))` | 5 |
| % | Modulus | `echo $((10 % 3))` | 1 |

### 🧩 Logical Operators
```bash
#!/bin/bash

a=10
b=25

if [ $a -gt 5 ] && [ $b -gt 20 ]; then
    echo "Both conditions are true"
fi
```

| Operator | Meaning |
|----------|---------|
| && | Logical AND |
| \|\| | Logical OR |
| ! | Logical NOT |

### 🧩 File Test Operators

```bash
#!/bin/bash

file="test.txt"
dir="myfolder"

# -e : file exists
if [ -e "$file" ]; then
    echo "$file exists"
fi

# -d : directory exists
if [ -d "$dir" ]; then
    echo "$dir is a directory"
fi

# -f : regular file
if [ -f "$file" ]; then
    echo "$file is a regular file"
fi

# -s : file is not empty
if [ -s "$file" ]; then
    echo "$file is not empty"
else
    echo "$file is empty"
fi

```
| Operator | Meaning |
|----------|---------|
| -e | Checks if a file exists |
| -d | Checks if a directory exists |
| -f | Checks if a file is a regular file |
| -s | Checks if a file is not empty |










