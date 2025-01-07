Reader be warned, I'm not very good at splitting these topics between "Bash" and general "Linux"...

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

OR

`readlink -f <filename>`


## Ping with Timestamp

`ping <ip_address> | while read pong; do echo "$(date -u +%T): $pong"; done`

Source: https://stackoverflow.com/questions/10679807/how-do-i-timestamp-every-ping-result

## The "Script" command

Use the [`script`](https://www.redhat.com/en/blog/linux-script-command) command to capture a terminal session to file and potentially replay with timing!

-  When you want to end and save the file, use Ctrl-D on your keyboard. 

https://www.redhat.com/en/blog/linux-script-command

## Terminals, Terminal Emulators, Pseudo Terminals

- https://man7.org/linux/man-pages/man1/stty.1.html
- https://www.computerhope.com/unix/ustty.htm
- https://blog.viktomas.com/graph/new-lines-and-terminals-cr-vs-lf/
