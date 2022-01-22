# PowerShell Cheatsheet


## Index

 * [Global regex search and replace](#Global-regex-search-and-replace)
   * [Search with directory exclude](#Search-with-directory-exclude)
   * [Search and replace](#Search-and-replace)
   * [Recursively remove specified directories](#Recursively-remove-specified-directories)
 * [File IO](#File-IO)
   * [Extract ZIP archive to specified folder](#Extract-ZIP-archive-to-specified-folder)
   * [Illform-encoded file fix](#Illform-encoded-file-fix)
 * [VS Code](#VS-Code)
   * [Open settings file](#Open-settings-file)
   * [Disable Integrated Console on Startup](#Disable-Integrated-Console-on-Startup)
   * [Unsing custom PowerShell version as default](#Unsing-custom-PowerShell-version-as-default)
   * [Disabling code lens for PowerShell](#Disabling-code-lens-for-PowerShell)


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

### Extract ZIP archive to specified folder

```powershell
Expand-Archive "$env:USERPROFILE\Downloads\ninja-win.zip" "$env:LOCALAPPDATA\Programs\ninja"
```

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

## VS Code

### Open settings file

Open `VS Code`, press `Ctrl+Shift+P`, type `Open Settings (JSON)`.

### Disable Integrated Console on Startup

```json5
{
    "powershell.integratedConsole.showOnStartup": false
}
```

### Unsing custom PowerShell version as default

```json5
{
    "terminal.integrated.shell.windows": "C:/Program Files/PowerShell/7/pwsh.exe"
}
```

### Disabling code lens for PowerShell

```json5
{
    "[powershell]": {
        "editor.codeLens": false
    }
}
```
