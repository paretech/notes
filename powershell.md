# Powershell Snippets

## Zip Logs

```
$path = "some\path"
$relDirPath = [io.path]::GetDirectoryName($path)
$dirName = [io.path]::GetFileName($relDirPath)
$timeStamp = $(get-date -f yyyy-MM-ddTHHMMss)
Compress-Archive $relDirPath "$($timeStamp) $($dirName)"
```
