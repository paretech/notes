## Debugging Scripts
- https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_02_03.html
- https://stackoverflow.com/questions/1378274/in-a-bash-script-how-can-i-exit-the-entire-script-if-a-certain-condition-occurs
  - Several good comments and sub-posts

## Get directory of currently running script

```
SCRIPT_DIRECTORY=$(dirname "$(readlink -f "$0")")
```

## Boot Systems
- https://www.oreilly.com/library/view/hands-on-booting-learn/9781484258903/
