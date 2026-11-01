### Pull from current repository

By default the runner has full read permission on current repository (using built-in GITHUB_TOKEN), regardless of private or not.

```yaml
jobs:
  JOB:
    steps:
      - name: Checkout repository
        uses: actions/checkout@v5
```

After checking out, CWD will be exactly at the root of repo.

### Push to current repository

To enable pushing, write permission must be enabled on GITHUB_TOKEN:

Repository "Settings" -> "Actions" -> "General" -> "Workflow permissions" -> Check on "Read and write permissions".

```yaml
jobs:
  JOB:
    steps:
      - name: Push changes
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add .
          git commit -m "updated by github-actions[bot]"
          git push
```