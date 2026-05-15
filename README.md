# Git line endings demo

This repo demonstrates how Git handles line endings between platforms.

Checking this out on Windows should have the following properties:

- `lf.txt` has Unix line endings
- `auto.txt` has the system line endings (CRLF on Windows, Unix elsewhere)

To verify this, run

```powershell
# Check for platform invariant Unix line endings
> (Get-Content -Path "lf.txt" -Raw) `
    -replace "`r`n", "[CRLF]`r`n" `
    -replace "(?<!`r)`n", "[LF]`n"
The[LF]
quick[LF]
brown[LF]
fox[LF]
jumped[LF]
over[LF]
the[LF]
lazy[LF]
dog[LF]

# Check for platform conformant line endings
> (Get-Content -Path "auto.txt" -Raw) `
    -replace "`r`n", "[CRLF]`r`n" `
    -replace "(?<!`r)`n", "[LF]`n"
The[CRLF]
quick[CRLF]
brown[CRLF]
fox[CRLF]
jumped[CRLF]
over[CRLF]
the[CRLF]
lazy[CRLF]
dog[CRLF]
```

On Linux, checking this out should have the following properties:

- `lf.txt` has Unix line endings
- `auto.txt` has Unix line endings

To verify this, run

```bash
# lf.txt has Unix line endings (represented by a '$' character)
$ cat -A lf.txt
The$
quick$
brown$
fox$
jumped$
over$
the$
lazy$
dog$

# auto.txt also has Unix line endings
$ cat -A auto.txt
The$
quick$
brown$
fox$
jumped$
over$
the$
lazy$
dog$
```

## Applying this to an existing repo with mixed line endings

If you have a repo with existing mixed line endings and you want to standardize on one or the other, you can do the following:

1. Make the required changes in the `.gitattributes` file
2. Run `git add --renormalize .` to force Git to re-evaluate all the files and stage any with line-ending changes for commit
3. Commit the changes: `git commit -m "Normalize all line endings"`

At this point, Git's view of the files will be correct as-per the `.gitattributes` settings but the files checked out in your local copy may still have their old line endings. To force an update, you can run:

```
# Remove all checked out files from workspace
$ git rm --cached -rf .

# Re-check them out, this time with correct line endings
$ git reset --hard HEAD
```
