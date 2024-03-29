# Powershell Snippets

## Zip Logs

```
$path = "some\path"
$relDirPath = [io.path]::GetDirectoryName($path)
$dirName = [io.path]::GetFileName($relDirPath)
$timeStamp = $(get-date -f yyyy-MM-ddTHHMMss)
Compress-Archive $relDirPath "$($timeStamp) $($dirName)"
```

## Extract all Zip files in folder (in parallel)

```
Get-ChildItem -Path .\ -Filter *.zip | ForEach-Object -Parallel {Expand-Archive $_}
```

See also https://devblogs.microsoft.com/powershell/powershell-foreach-object-parallel-feature/


## Find Files with Size Greater Than

```
Get-ChildItem 'E:\music_smaller\' -recurse | Where-Object {$_.length -gt 20*1024*1024} | Sort-Object length | Format-Table FullName, Size
```

For more tips on finding things, see Doctor Scripto's blog post, https://devblogs.microsoft.com/scripting/use-windows-powershell-to-search-for-files/

## Get Full File Path
```
(Get-Command oh-my-posh).Source
```

## "Touch" a File
```
New-Item -Path $PROFILE -Type File -Force
```
## Reload a File
```
. $PROFILE
```

## Display all environment variables

```
dir env:
```

Should be noted that `dir` is an alias to `Get-ChildItem`.

Source: https://devblogs.microsoft.com/scripting/powertip-use-windows-powershell-to-display-all-environment-variables/

## Access value of environment variable

```
$env:OneDrive
```

Source: https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_environment_variables?view=powershell-7.3

## Make Powershell home with Windows Terminal and Oh-My-Posh

- https://github.com/microsoft/terminal
- https://www.hanselman.com/blog/my-ultimate-powershell-prompt-with-oh-my-posh-and-the-windows-terminal
- https://www.hanselman.com/blog/spend-less-time-cding-around-directories-with-the-powershell-z-shortcut


## Delete Directory

```
Remove-Item .\scratch\ -Recurse -Force
```


## List Files in Directory

I find I'm frequently getting list of files and folders to paste into documents. This use to be a flag away in CMD (e.g. `dir /b`). Can get something equivalent for PowerShell. Note that `gci` is an alisas for `Get-ChildItem`.

```
(gci).Name | clip
```


## Get Path of Commandlet

```
(Get-Command wt).Path
(gcm wt).Path
```

Source: https://superuser.com/a/676107
