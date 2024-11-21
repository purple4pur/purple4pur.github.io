### Quick reference

```
| type | [u]ser  | [g]roup | [o]thers |
| ---- |  - - -  |  - - -  |  - - -   | # Regular permissions
|    - |  r      |  r      |  r       |   - can be read
|    - |    w    |    w    |    w     |   - can be modified
|    - |      x  |      x  |      x   |   - can be executed
|    d |  r      |  r      |  r       |   - contents can be shown
|    d |      x  |      x  |      x   |   - can be accessed with 'cd'
|    d |    w x  |    w x  |    w x   |   - contents can be modified
| ---- |  - - -  |  - - -  |  - - -   | # Special permissions
|  -/d |      s  |         |          |   - setuid w/ 'x'
|    - |      S  |         |          |   - setuid w/o 'x' (rare)
|    d |      S  |         |          |   - (useless)
|  -/d |         |      s  |          |   - setgid w/ 'x'
|    - |         |      S  |          |   - setgid w/o 'x' (rare)
|    d |         |      S  |          |   - (useless)
|  -/d |         |         |      t   |   - sticky w/ 'x'
|    - |         |         |      T   |   - sticky w/o 'x' (rare)
|    d |         |         |      T   |   - sticky w/o 'x'
```

### Setuid (SUID)

> `stat = 4---`

Will be executed **as the file owner**.

- **/usr/bin/passwd**: `-rwsr-xr-x root root`

### Setgid (SGID)

> `stat = 2---`

Files/Directories created within the folder or by the file will **have the same group ownership** as it, and inherit this SGID bit.

### Sticky

> `stat = 1---`

Not allowed to be removed.

- **/tmp**: `drwxrwxrwt root root`

### Appendix: links

- [[File permissions and attributes - ArchWiki]](https://wiki.archlinux.org/title/File_permissions_and_attributes)
- [[setuid - Wikipedia]](https://en.wikipedia.org/wiki/Setuid)
- [[Sticky bit - Wikipedia]](https://en.wikipedia.org/wiki/Sticky_bit)