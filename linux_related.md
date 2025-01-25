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
  - The stty command is used to configure and display terminal line settings, allowing users to control how input and output are handled by the terminal.
  - Line endings, see options ICRNL, INLCR, and IGNCR
- https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap11.html
  - For better understanding of line endings, see relevant sections under "General Terminal Interface" and "Input Modes" (ch 11).
- https://www.computerhope.com/unix/ustty.htm

## USB Wifi Network Adapters

- https://github.com/morrownr/USB-WiFi?tab=readme-ov-file
- https://blog.viktomas.com/graph/new-lines-and-terminals-cr-vs-lf/
- The term "new line" is more of a conceptual representation of moving to the start of the next line. It can be represented by different character sequences depending on the system (e.g. LF, CR, CRLF). See resources above for additional context.
