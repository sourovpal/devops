# Git & Github Command

### 🧩 Install Git

```bash
    👉 sudo apt update
    👉 sudo apt install git -y
    👉 git --version
```

### 🧩 Git Config

```bash
    👉 git config --global user.name "Sourov Pal"
    👉 git config --global user.email "sourovpal@gmail.com"
```

### 🧩 Git Init

```bash
    👉 git init
    👉 git status
```

### 🧩 Git Add/Stage

```bash
    👉 git add .              # সব ফাইল add করতে
    👉 git add index.html    # নির্দিষ্ট ফাইল add করতে
    👉 git add index.html style.css # Multiple file যুক্ত করতে
    👉 git add -A             # বর্তমান ফোল্ডার + সাবফোল্ডারের সব পরিবর্তন add করে
    👉 git add --all          # সব changes (new, modified, deleted) add করে
    👉 git add src/           # নির্দিষ্ট ফোল্ডার add করতে
    👉 git add -u             # শুধু modified (tracked) ফাইল add করতে
    👉 git add -i             # Interactive / Selective add
    👉 git add -p             # menu দিয়ে ফাইল সিলেক্ট

    👉 git add -n .           # কি add হবে তা দেখার জন্য (dry run)

    # ফাইল add করা বাতিল করতে (unstage)

    👉 git restore --staged file.txt
    👉 git reset file.txt
```

### 🧩 Git Unstage

```bash
    👉 git restore --staged .              # ✅ Safe! সব staged ফাইল unstage করবে
    👉 git reset                           # ✅ Safe!
    👉 git reset HEAD                      # ✅ Safe!
    👉 git restore --staged file.txt       # ✅ Safe! নির্দিষ্ট ফাইল unstage
    👉 git reset --hard                    # ❌ Dangerous! Changes delete করে
```

### 🧩 Git Commit

```bash
    👉 git commit -m "message"     # সাধারণ commit
    👉 git commit -am "message"    # Add + Commit একসাথে
    👉 git commit                  # Commit message editor open
    👉 git commit --amend          # Previous commit amend করতে বা সাথে যুক্ত করতে
    👉 git commit --amend -m "new message"
    👉 git commit file.txt -m "message"             # Specific file commit
    👉 git commit --allow-empty -m "empty commit"   # Empty commit
    👉 git commit --no-edit        # Commit না করে message edit
    👉 git commit --no-verify      # Hooks skip করে commit (pre-commit hook skip)
    👉 git commit --author="Name <email@example.com>" # Author সহ commit
    👉 git commit --date="2025-01-01 10:00:00"        # Date সহ commit
    👉 git commit -v        # Verbose commit
    👉 git commit -C HEAD   # Reuse previous commit message [-C or -c]
    👉 git commit -m "Title" -m "Description"      # Multiple message lines
```










