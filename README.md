# Tag And Warp (a.k.a. `warpdrive`)

Easily tag locations in *deep space* (directories) and easily *warp* (like the *space drive*) to them with a few keystrokes.

---

## Installation

Installation is quite straightforward:
1) Copy the file `[path-to-repo]/src/.warpdrive` to `[some-location]`;
2) Depending on the shell you are using (`bash` or `zsh`), add the following line to `~/.bashrc` and/or `~/.zsh` respectively:
<br>`source "[some-location]/.warpdrive"`

---

## Usage

The script enables the following shell commands, and respective autocompletion.

### tag

Tags a directory with a tagname and adds it to the taglist.

```bash
tag [tagname] [dirpath]
```
`[tagname]` and `[dirpath]` are **both** optional arguments:

- If no argument is specified, the current directory will be tagged with its own name as tagname;
- If only one argument is passed, it is assumed to be a `[tagname]` and the current directory will be tagged with such name;
- If both arguments are passed, the full path `[dirpath]` will be tagged as `[tagname]`.

### untag

Removes a tag from the taglist.
```bash
untag [tagname]
```
`[tagname]` is an optional argument. If no tagname is specified, and you are in a tagged directory, it will automatically remove such directory from the taglist.

### warp

Moves to the directory whose tag is given.

```bash
warp [tagname]
```
Equivalent to `cd [tagged-directory]` with `[tagged-directory]` the directory identified by the tag.
<br>In this case, `[tagname]` is a mandatory argument.

### retag

Rename an existing tag in the taglist.

```bash
retag [tagname] newname
```

`[tagname]` is an optional argument, whereas `newname` must be provided. <br>If you are in a tagged directory, and only one argument is passed, the tag associated to current directory will be renamed to the tagname provided.

Additional examples:

```bash
retag mydir           # retags current directory, if tagged, to `mydir`
retag thisdir mydir   # retags `thisdir` to `mydir`
```

### tags

Lists all tags in the taglist.

---

## Refactoring notes and caveats

The master branch of this repository contains my personal take (and subsequent refactoring) of the original `.warpdrive` script. And, as strange as it seems, it does **not** provide full backwards compatibility with it.

In particular, both the `untag` and `retag` functions has been implemented (compatibly between themselves) differently, as explained.
<br>The original script assumes the tag of current directory (if eixstent) as the optional tag **both**
1) when the optional tag has not been specified by the user;
2) when the optional tag has been explicitly specified by the user **but** it does not exist.

The refactored version of the script does this only in case 1. , failing in case 2. .

Another breaking change is present with regards to multiply-tagged directories.
<br> The script does allow to tag the same directory with multiple, differently-named tags. When invoking the `untag` command with no arguments, from a directory among these, the **last** tag in alphabetical order is selected as the one to be removed.

The refactored version replicates this behaviour for the sake of command essentiality, but deleting the **first** of the tags, in alphabetical order.

This may be useful - at the cost of additional failures - to guard the user against accidental (existent) tag deletions when invoking nonexistent tag deletion from a tagged directory.

The `tag` function has been enhanced in a non-breaking way. It is now possible to tag directories while not being inside them, just by passing the full path as a second argument. The one-argument version is fully backwards-compatible.

This allows, at the cost of a warning, to even tag non-already-existing directories, which may be useful in scripts or with automatically-generated paths.

The remaining, minor, changes are non-breaking and mainly revolve around improved robustness against paths with spaces, improved documentation, better compatibility with shells installed in non-standard locations, typesetting enhancements

---

## Credits

Originally based on Jeroen Janssens' `jumper`.

Original codebase by Joeri Hermans
<br>Retag functionality by Benjamin Shanahan
