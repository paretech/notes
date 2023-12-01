## Debugging Scripts
- https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_02_03.html
- https://stackoverflow.com/questions/1378274/in-a-bash-script-how-can-i-exit-the-entire-script-if-a-certain-condition-occurs
  - Several good comments and sub-posts
- `strace`
  - https://jvns.ca/strace-zine-v2.pdf
  - https://www.youtube.com/watch?v=Rryi_1JZnuc

## Get directory of currently running script

```
SCRIPT_DIRECTORY=$(dirname "$(readlink -f "$0")")
```

## Boot Systems
- https://www.oreilly.com/library/view/hands-on-booting-learn/9781484258903/

## Frequent Commands

### Compress Archive
`tar -czvf archive.tar.gz files`

### Decompress Archive
`tar -xzvf archive.tar.gz`

### Get full path of file

`realpath rootfs.cpio.gz`
