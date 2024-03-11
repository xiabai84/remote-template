# Setup remote helm-repository

### Pre-requisites

1. A GitHub repo containing a directory with your Helm charts (default is a folder named `/charts`, if you want to
   maintain your charts in a different directory, you must include a `charts_dir` input in the workflow).
2. A GitHub branch called `gh-pages` to store the published charts. 
   ```
   $ git checkout -b gh-pages
   $ git push --set-upstream origin gh-pages
   ```
3. In your repo, go to Settings/Pages. Change the `Source` `Branch` to `gh-pages`.
4. Create a workflow `.yml` file in your `.github/workflows` directory.
   ```
    name: Release Chart
    on:
      push:
        branches:
          - main
    
    jobs:
      release:
        permissions:
          contents: write
        runs-on: ubuntu-latest
        steps:
        - name: Checkout
          uses: actions/checkout@v4
          with:
            fetch-depth: 0

        - name: Configure Git
          run: |
            git config user.name "$GITHUB_ACTOR"
            git config user.email "$GITHUB_ACTOR@users.noreply.github.com"

        - name: Install Helm
          uses: azure/setup-helm@v4
          with:
            version: v3.8.1

        - name: Run chart-releaser
          uses: helm/chart-releaser-action@v1.6.0
          env:
            CR_TOKEN: "${{ secrets.GITHUB_TOKEN }}"
   ```

### Add remote helm repository
```bash
helm repo add remote-template https://xiabai84.github.io/remote-template