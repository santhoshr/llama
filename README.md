# 🦙 llama File Manager

Llama — a terminal file manager.

<table>
<tr>
<th> Bash </th>
<th> Fish </th>
<th> PowerShell </th>
</tr>
<tr>
<td>

```bash
function ll {
	cd "$(llama "$@")"
}
```

</td>
<td>

```fish
function ll
cd (llama $argv);
end
```

</td>
<td>

```powershell
function ll() {
	cd "$(llama $args)"
}
```

</td>
</tr>
</table> <br/>
<table>
	<tr>
<th>Powershell Improved</th>
	</tr><tr> 
<td> 
	
```powershell improved
function ll {
    $output = & llama @args

    # Show raw output for debugging
    # $output | ForEach-Object { Write-Host "`t$_" }

    foreach ($line in $output) {
        if (-not [string]::IsNullOrWhiteSpace($line)) {
            # Trim leading/trailing spaces
            $trimmed = $line.Trim()

            # Try to resolve relative path to full path
            try {
                $resolvedPath = Resolve-Path -Path $trimmed -ErrorAction Stop
                Set-Location $resolvedPath
                return
            } catch {
                # Ignore invalid lines
            }
        }
    }

    Write-Host "No valid path returned from llama." -ForegroundColor Red
}

```

</td>
</tr>
</table>


Note: we need a such helper as the child process can't modify the working
directory of the parent process.

## Usage

 | Key binding        | Description                                                                   | 
 | ------------------ | --------------------                                                          | 
 | `Arrows`, `hjkl`   | Move cursor                                                                   | 
 | `Shift + Arrows`   | Move to the corners                                                           | 
 | `g`, `G`           | Move to the top or bottom                                                     | 
 | `Enter`, `L`       | Enter directory                                                               | 
 | `Backspace`, `H`   | Exit directory                                                                | 
 | `Esc`              | Exit with cd if empty search / filter otherwise empties search / filter       | 
 | `Ctrl+C`           | Exit without cd                                                               | 
 | `dd`, `DD`         | Delete file or dir, use capital when search / filter is active                | 
 | `/`                | Fuzzy search                                                                  | 
 | `r`  , `R`         | Reload directory, use capital when in search / filter is active               | 
 | `f, F`             | Toggle Filter, use capital when search / filter is active                     | 
 | `tab`              | Toggle between search and filter and between on and off by double pressing it | 
 | `space`            | Toggle Search / Filter on and off                                             | 
 | `\`                | Navigate to Root                                                              | 
 | \``, ~`            | Navigate to Home                                                              | 
 | `!`                | Toggle files filtering directories                                            | 
 | `@`                | Toggle directories filtering files                                            | 

The `EDITOR` or `LLAMA_EDITOR` environment variable used for opening files from
the llama.

```bash
export EDITOR=vim
```

## License

[MIT](LICENSE)

