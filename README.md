name: Create Daily Repository

on:
  schedule:
    - cron: '0 0 * * *' # Запускать каждый день в полночь

jobs:
  create_repo:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v2

      - name: Create Repository
        run: |
          TOKEN=$1
          REPO_NAME="new-repo-$(date +\%Y\%m\%d)"
          curl -X POST -H "Authorization: token $TOKEN" https://api.github.com/user/repos -d '{"name":"'$REPO_NAME'"}'

        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
