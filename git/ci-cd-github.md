

### Simple html page ci/cd configer
`.github/workflows`
```yaml
# SSH_HOST_NAME
# SSH_HOST_IP
# SSH_PRIVATE_KEY
# <repo-url>/settings/secrets/actions    Save all env

# Multiple Branch trigger
  # - main
  # - dev
  # - staging

# All Branch trigger
  # - "*"


name: Deploy via SSH

on:
  push:
    branches:
      - main  # trigger on main branch

jobs:
  deploy:
    runs-on: ubuntu-latest        # এখানেই নির্ধারিত হয় workflow কোন environment বা machine-এ রান হবে।

    steps:
      - name: Checkout code
        uses: actions/checkout@v3            # GitHub repository-এর code copy হয়ে আসে, যাতে পরের steps ব্যবহার করতে পারে।

      - name: Setup SSH                      # এই step runner-এ SSH agent চালু করে এবং secret থেকে private key load করে দেয়, যাতে পরের steps remote server-এ password-less SSH/rsync করতে পারে।
        uses: webfactory/ssh-agent@v0.8.1
        with:
          ssh-private-key: ${{ secrets.SSH_PRIVATE_KEY }}

      - name: Deploy files via rsync
        run: |
          rsync -avz --delete ./ ${{ secrets.SSH_HOST_NAME }}@${{ secrets.SSH_HOST_IP }}:/var/www/html/

# Run SSH Multiple Script
/*
- name: Install Node & Build Frontend
    uses: appleboy/ssh-action@v1.0.3
    with:
      host: ${{ secrets.LINUX_HOST }}
      username: ${{ secrets.LINUX_USER }}
      key: ${{ secrets.LINUX_SSH_KEY }}
      script: |
        cd ${{ secrets.LINUX_DIR_PATH }}

        echo "📦 Installing Node packages..."
        npm install --production
*/
```
