![image](https://github.com/user-attachments/assets/0acbe57f-10d5-4a61-986f-3663840164a7)# Powershell Resources
- https://learn.microsoft.com/en-us/powershell/scripting/learn/ps101/00-introduction

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


## Zip All Folders (synchronous)

```PowerShell
function Zip-Folders {
    param (
        [string]$InputDirectory = (Get-Location).Path,
        [string]$OutputDirectory = (Get-Location).Path
    )
    # Get all directories within the input directory
    $folders = Get-ChildItem -Path $InputDirectory -Directory
    # Process each folder serially
    foreach ($folder in $folders) {
        # Define the path for the output zip file
        $zipFilePath = Join-Path -Path $OutputDirectory -ChildPath "$($folder.Name).zip"
        # Create the zip file
        [System.IO.Compression.ZipFile]::CreateFromDirectory($folder.FullName, $zipFilePath)
        Write-Host "Zipped $($folder.Name) to $zipFilePath"
    }
}
```

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

## Create a local index of a directory

This snippet is really useful when you are on a new team, especially one with a SLOW disorganized LAN drive...

```
Get-Childitem –Path "\\network_path" -Recurse -File –Force -ErrorAction SilentlyContinue |
    Select-Object -ExpandProperty FullName |
    Tee-Object -FilePath ~\filelist.txt
```

## "Touch" a File
```
New-Item -Path $PROFILE -Type File -Force
```

You can expand this further by defining a function and adding an alias in your `$PROFILE` file. For example, add the following.

```powershell
function Touch-File {
    param (
        [string]$Path
    )
    if (-Not (Test-Path $Path)) {
        # Create a new file
        New-Item -ItemType File -Path $Path -Force | Out-Null
    } else {
        # Update the last modified timestamp
        (Get-Item $Path).LastWriteTime = Get-Date
    }
}
# Create an alias for convenience
Set-Alias touch Touch-File
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
  - https://github.com/badmotorfinger/z?tab=readme-ov-file
    - https://www.powershellgallery.com/packages/z/1.1.14
- https://ohmyposh.dev/docs/installation/windows#next
- Install 'z'
  - https://www.hanselman.com/blog/spend-less-time-cding-around-directories-with-the-powershell-z-shortcut
  - https://github.com/badmotorfinger/z
  - https://www.powershellgallery.com/
    - https://www.powershellgallery.com/packages/z/1.1.14


## Delete Directory

```
Remove-Item .\scratch\ -Recurse -Force
```


## List Files in Directory

I find I'm frequently getting list of files and folders to paste into documents. This use to be a flag away in CMD (e.g. `dir /b`). Can get something equivalent for PowerShell. Note that `gci` is an alisas for `Get-ChildItem`.

```
(gci).Name | clip
```


## Get Path of Commandlet (like which or where)

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

This method can also be used to remove specific items from history which prevents them from appearing in autocomplete.


## Get File Hash

```
Get-FileHash zaber-motion-lib-linux-amd64.so -Algorithm SHA512 | Format-List
```

Source: https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-filehash?view=powershell-7.4

Get file hash for all files in directory

```powershell
Get-ChildItem -Path .\ | ForEach-Object {Get-FileHash -Algorithm SHA1 $_}
```


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

## Enable NTFS Long Paths (Windows Long File Names)
```
New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" `
-Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force
```

Check to make sure enabled

```
Get-ItemPropertyValue -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name "LongPathsEnabled"
```

See also 
- https://stackoverflow.com/questions/21194530/what-does-mean-when-prepended-to-a-file-path
- https://learn.microsoft.com/en-us/windows/win32/fileio/naming-a-file?redirectedfrom=MSDN#win32-file-namespaces
- https://learn.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation?tabs=registry

## Get Powershell Version
```
$PSVersionTable.PSVersion
```

Source: https://stackoverflow.com/questions/1825585/determine-installed-powershell-version

## Get Available Items for Commands
"`Get-Member` helps you discover what objects, properties, and methods are available for commands."

```
$(Get-Item -Path my_file.txt) | Get-Member

Get-Item -Path my_file.txt | Select-Object -Property *
```

Source: https://learn.microsoft.com/en-us/powershell/scripting/learn/ps101/03-discovering-objects


## "Tail" a file

```
Get-Content -wait -Path <file_path>
```

Source: https://stackoverflow.com/questions/4426442/unix-tail-equivalent-command-in-windows-powershell

## "Grep" output using Select-String

```
dir env: | Select-String -Pattern PATH
```

## List USB Devices

```
Get-PnpDevice -PresentOnly | Where-Object { $_.InstanceId -match '^USB' }
```
			
From <https://imagescience.com.au/knowledge/checking-usb-device-connections>

## Current Working Directory

`(Get-Location).path | clip` or `$PWD.path | clip`

## Path Manipulation

Get the name of the parent or current working directory.

```
$directoryName = Split-Path -Path (Get-Location) -Leaf
```

## Building and Formatting Strings

https://learn.microsoft.com/en-us/powershell/scripting/learn/deep-dives/everything-about-string-substitutions?view=powershell-7.5

## Style

"When naming your functions in PowerShell, use a Pascal case name with an approved verb and a singular noun. To obtain a list of approved verbs in PowerShell, run Get-Verb. "

Source: https://learn.microsoft.com/en-us/powershell/scripting/learn/ps101/09-functions?view=powershell-7.


## Building Complex File Lists

Sometimes you need to build a list of files from a folder containing folders with similar folder structure. Here is an example of how to extract a list of images satisfying certain conditions from within that structure

```
$fancy_image_list = (Get-ChildItem -Path $BasePath -Recurse -Filter "*-1-*.png" | Where-Object {$_.FullName -match '\\images\\type1\\'} | ForEach-Object {$_.FullName}) -join " "
```
