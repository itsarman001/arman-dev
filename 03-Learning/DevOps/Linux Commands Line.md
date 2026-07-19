# Linux Commands — Notes

## Structure of a Linux Command

Generally, a command has three parts:

```
Name + Options + Arguments
```

Example:

```
cat -n file.txt        (short form)
cat --number file.txt  (long form)
 |     |         |
Name Option   Argument
```

- `-n` and `--number` are two ways of passing the **same** option to `cat` (numbers each line of output):
    - `-n` → short form
    - `--number` → full/long name
- `file.txt` is the **argument**.

There can be an unlimited number of options and arguments passed to a command (in practice). There _is_ a maximum, but it's imposed by the operating system, not by the shell/command itself.

> **Options and arguments are case-sensitive.**

---

## Summary

- `Command = command name + options + arguments`
- Options are case-sensitive
- Options can be combined (grouped)
- Option short form: `cat -n abc.txt`
- Option long form: `cat --number abc.txt`

---

## `man` Command

Used to read the manual for any command.

In the **SYNOPSIS** section you'll find:

```
command_name options arguments
```

Example — `cp`:

```
cp [OPTION] [-T] SOURCE DEST
    -------      ------
    optional     required
```

- Wrapped in `[ ]` → **optional**
- **Underlined** → **required**

---

## `ls`, `pwd`, `cd`

- **`pwd`** — Print Working Directory
- **`cd`** — Change Directory
- **`ls`** — List directory contents
    - `ls -a` / `ls --all` → list directory contents _including hidden files_

### Combining (grouping) options

Options can be merged for shorthand, e.g.:

```
ls -at     # sort by time, and show hidden files
```

### Directory references

- `.` → current directory
- `..` → parent (previous) directory
- `/` → root of the filesystem
- `~` → home directory

---

## Timestamps

|Command|Shows|Updated when...|
|---|---|---|
|`ls -l`|Modification time|The **contents** of the file are changed|
|`ls -lc`|Change time|The file is **moved, renamed**, or its **permissions** change|
|`ls -lu`|Access time|The file has been **accessed** by any means (terminal, app, etc.)|

---

## Making Directories and Files

### `mkdir` — create directories

```bash
mkdir folder_name              # create a single folder
mkdir -p parent/child          # create nested folders without errors
mkdir -p parent/{child1,child2}
mkdir -p a/b/c a/{b1,b2}
```

- `-p` creates parent directories as needed, and won't error if they already exist.

### `touch` — create files

```bash
touch a.txt b.txt
```

- `touch` updates **both** access time and modification time of an existing file — unless you use:
    - `-a` → change **only** access time
    - `-m` → change **only** modification time

### `stat`

```bash
stat <entity>
```

Shows detailed status/metadata of a file or directory (timestamps, size, permissions, etc.)

### Files/folders with spaces in the name

Wrap the name in quotes:

```bash
touch "a b"
mkdir "a b"
```

---

## `file` Command

`file <filename>` ignores the file's **extension** and instead inspects the file's **actual content** to determine its real type.

Example:

```bash
touch file.mp4        # actually contains a Python script
file file.mp4
# → file.mp4: Python script, ASCII text executable
```

So `file` reports what the content _actually is_, regardless of what the extension claims.