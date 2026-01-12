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

## 🧩 Bash Operators

#### 🧩 Comparison Operators
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














