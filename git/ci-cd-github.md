

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
        uses: actions/checkout@v3

      - name: Setup SSH
        uses: webfactory/ssh-agent@v0.8.1
        with:
          ssh-private-key: ${{ secrets.SSH_PRIVATE_KEY }}

      - name: Deploy files via rsync
        run: |
          rsync -avz --delete ./ ${{ secrets.SSH_HOST_NAME }}@${{ secrets.SSH_HOST_IP }}:/var/www/html/
```
