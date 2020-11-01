# PowerShell Cheatsheet


## Index

 * [Global regex search and replace](#Global%20regex%20search%20and%20replace)
   * [Search with directory exclude](#Search%20with%20directory%20exclude)
   * [Search and replace](#Search%20and%20replace)
   * [Recursively remove specified directories](#Recursively%20remove%20specified%20directories)
 * [File IO](#File%20IO)
   * [Illform-encoded file fix](#Illform-encoded%20file%20fix)


## Global regex search and replace

### Search with directory exclude

Searches all JavaScript files for classes that begin with `Test`, excluding `node_modules` dir:

```powershell
gci -Recurse -Include *.js | ?{ $_.FullName -inotmatch 'node_modules' } | Select-String '\bclass\s+Test'
```

### Search and replace

Replaces any licenses within `package.json` files with `MIT` (original files will be overwritten):

```powershell
gci -Recurse -Filter package.json | Select-String '"license": ".+?"' -List | %{
    ((gc $_.Path -Raw -Encoding utf8) -replace '"license": ".+?"', '"license": "MIT"') | Out-File $_.Path -NoNewline -Encoding utf8 }
```

Remarks:
 * The `-List` option will group multiple matches within one file into single result.
 * The `-Raw` option will read entire file contents as one string.
 * The `-NoNewline` option will not add new line symbols to the end of the file.


### Recursively remove specified directories

Recursively scans and removes specified directories (`build` and `node_modules`):

```powershell
gci -Recurse -Include build, node_modules -Directory | rmdir -Recurse -Force
```

## File IO

### Illform-encoded file fix

`UTF16LE` illformed file fix (removes `BOM`, zero bytes and `CR`-only line breaks):

```powershell
$CR = [byte][char]"`r"
$NL = [byte][char]"`n"
$bytes = [io.file]::ReadAllBytes('resource.h')
$bytes_fixed = [collections.generic.list[byte]]::new()
for ($i = 2; $i -lt $bytes_in.Length; $i++) {
    $byte = $bytes[$i]
    if ($byte -ne 0) {
	if (($byte -eq $CR) -and ($bytes[$i + 1] -ne $NL)) {
            continue
	}
        $bytes_fixed.Add($byte)
    }
}
[io.file]::WriteAllBytes('resource_fixed.h', $bytes_fixed)
```
