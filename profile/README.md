# reporeq

The simplest and last package manager you will ever use

## Usage

```bash
cd bin/
./reporeq              # reads reporeq.txt
./reporeq custom.txt   # reads custom.txt
```

## Format

`reporeq.txt` - one repo per line:

```
https://github.com/user/repo@v1.0.0
https://github.com/user/other@main
https://github.com/user/repo@abc123def
https://github.com/user/repo@feature/branch
git@gitlab.com:org/private.git@v2.0.0
```

- `URL@TAG` - clone at specific tag/branch/sha
- `URL` alone - clone HEAD
- Lines starting with `#` are comments
- Empty lines are ignored

## What It Does

1. Reads file line by line (order matters!)
2. Clones each repo to a temp directory
3. If ALL succeed → atomic move to `.reporeq/`
4. If ANY fail → abort, nothing changes
5. Links repo `bin/` executables to `.reporeq/bin/`

## Overlay Feature

Later entries overwrite earlier ones with the same name:

```
https://github.com/original/tool@v1.0.0
https://github.com/myfork/tool@patched     # overwrites!
```

Use this for patching or customizing upstream repos.

## Adding to PATH

```bash
export PATH="$PWD/.reporeq/bin:$PATH"
```

## Philosophy

Git already handles versioning, distribution, and history. Package managers reinvented all of this. This script just uses git directly. No dependencies beyond git and bash.

## Cross-Platform

Works on:
- Linux ✓
- macOS ✓  
- WSL ✓
- Git Bash (Windows) ✓

## Security Note

Only executables in each repo's `bin/` directory are shimmed. This reduces the risk of shadowing system commands. Still, only clone repos you trust. Avoid repos with scripts named `ls`, `cd`, `test`, etc. that could shadow system commands.

## License

GPLv3
