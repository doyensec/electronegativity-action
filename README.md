# Electronegativity GitHub Action

The action integrates [Electronegativity](https://github.com/doyensec/electronegativity), a tool to identify misconfigurations and security anti-patterns in Electron applications, into GitHub CI/CD.
It produces a GitHub compatible SARIF file for uploading to the repository 'Code scanning alerts'.

## Usage examples

```yaml
on: 
  push:
    
jobs:
  build_job:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - uses: actions/setup-node@v2
        with:
          node-version: '12'

      - uses: jarlob/electronegativity-action@v1.0

      - name: Upload sarif
        uses: github/codeql-action/upload-sarif@v1
        with:
          sarif_file: ../results
```

## FAQ

### Q:
I'm getting `checkPermissions Missing write access to /usr/local/lib/node_modules`
### A:
Add the following lines in your workflow before the action:

```yaml
- uses: actions/setup-node@v2
  with:
    node-version: '12' # or the node version you need
```

See [https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally](https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally) for other possible solutions.

### Q:
I'm running into the `Fatal Error JavaScript heap out of memory`
### A:
Specify additional memory with node arguments:

```yaml
- uses: jarlob/electronegativity-action@v1.0
  with:
    node-args: "--max-old-space-size=4096"
```
