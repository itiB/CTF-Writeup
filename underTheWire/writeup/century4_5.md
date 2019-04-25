# Century4->5

## 概要

The password for Century5 is the name of the file within a directory on the desktop that has spaces in its name.

NOTE:

- The password will be lowercase no matter how it appears on the screen.

## 解法

デスクトップの中身をみてそこにあったディレクトリに入ればいいらしい

```Powershell
> ls


    Directory: C:\Users\century4\Desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        8/30/2018   3:29 AM                Can You Open Me


PS C:\Users\century4\Desktop> cd '.\Can You Open Me'
PS C:\Users\century4\Desktop\Can You Open Me> ls


    Directory: C:\Users\century4\Desktop\Can You Open Me


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018   3:29 AM             24 61580
```

## FALG

- `ssh century5@century.underthewire.tech`  
- 61580
