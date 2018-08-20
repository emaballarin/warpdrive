# Tag And Warp (a.k.a. `warpdrive`)

Easily tag locations in *deep space* (directories) and easily *warp* (like the *space drive*) to them with a few keystrokes.
<br>Now refactored (`v. 0.2+`) *a.k.a. overengineered*, but still stable.

**NOTICE:** While this *piece of software* shares the name, and the original codebase, with the [*original* `warpdrive`](https://github.com/JoeriHermans/warpdrive), it has been heavily refactored in its internals. The features has been preserved as similar to the original as possible, but some breaking changes are present nevertheless.

---

## Installation

Installation is quite straightforward:
1) Copy the file `[path-to-repo]/src/.warpdrive` to `[some-location]`;
2) Depending on the shell you are using (`bash` or `zsh`, or both), add the following line to `~/.bashrc` and/or `~/.zsh` respectively:
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

If the command requires the creation of an already-existing tag (i.e. with the same name), it fails.
<br> It is possible (with a warning) to create tags pointing to *not-yet-existing* directories, which may be useful in some specific cases.

### untag

Removes a tag from the taglist.
```bash
untag [tagname]
```
`[tagname]` is an optional argument. If no tagname is specified, and you are in a tagged directory, it will automatically remove such directory from the taglist.

If such tag does not exist, the command fails.

### retag

Rename an existing tag in the taglist.

```bash
retag [tagname] <newname>
```

`[tagname]` is an optional argument, whereas `<newname>` is supposed to be provided:

- If no argument is provided, it is assumed that the user wanted to invoke the `tag` command instead of `retag`, and the former is executed as is (i.e. with no arguments).
- If an argument is passed:
  - If you are in a tagged directory, the tag associated to current directory will be renamed to the tagname provided.
  - If you are un an untagged directory, the command is executed as `tag` with one argument, thus tagging the current directory with given tagname.
- If both arguments are passed, the first (if existing) tag is renamed with the second tagname. If the first tag does not exist, the command fails.
-
If the command requires the creation of an already-existing tag (i.e. with the same name), it fails.


### warp

Moves to the directory whose tag is given.

```bash
warp [tagname]
```
Equivalent to `cd [tagged-directory]` with `[tagged-directory]` the directory identified by the tag.
<br>In this case, `[tagname]` is a mandatory argument.

### tags

Lists all tags in the taglist.

---

## Undocumented breaking changes

Apart from the behaviour explained above (which may differ with the original, `0.1.x`, versions of `warpdrive`), and unexposed internal reworkings, the refactored implementation of this script does not differ from the original, **with the sole exception** of the following caveat (undocumented in the original version).

If **a directory is referenced by multiple tags** and it is asked to **remove the tag associated with it**:
- In the *original version*, the tag whose name is **last** in alphabetic order is removed.
- In the *refactored version*, the tag whose name is **first** in alphabetic order is removed.

---

## Credits

Originally based on Jeroen Janssens' `jumper`.

Original codebase by Joeri Hermans
<br>Original retag functionality by Benjamin Shanahan
