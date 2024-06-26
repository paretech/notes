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

## Get Full File Path (of a command)
```
(Get-Command oh-my-posh).Source
```

## Get Full File Path (of a file)
```
(Resolve-Path .\chess3.png).path | clip
```

## Get Full File Path of all files in directory
```
Get-ChildItem -Path .\2.15.4\ -File -Recurse | Select-Object -ExpandProperty FullName
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

## Get History

```
subl (Get-PSReadlineOption).HistorySavePath
```

Source: https://stackoverflow.com/questions/44104043/how-can-i-see-the-command-history-across-all-powershell-sessions-in-windows-serv


## Get File Hash

```
Get-FileHash zaber-motion-lib-linux-amd64.so -Algorithm SHA512 | Format-List
```

Source: https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-filehash?view=powershell-7.4


## Get the target path of a symlink

Sometimes when debugging, you need to get the target path of a symlink. Here are a couple of examples...

```
# Get Path by pipe
Get-Item -Path .\cv2.pyd | Select-Object Target

# Get Path by attribute access
$(Get-Item -Path .\cv2.pyd).Target

# Get Absolute Path
$(Resolve-Path -Path $(Get-Item -Path .\cv2.pyd).Target).Path

# Change directory to parent
Set-Location $(Split-Path -Path $(Resolve-Path -Path $(Get-Item -Path .\cv2.pyd).Target) -Parent)

# Check if target exists
Test-Path $(Resolve-Path -Path $(Get-Item -Path .\cv2.pyd).Target)
```

## Enable NTFS Long Paths
```
New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" `
-Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force
```
