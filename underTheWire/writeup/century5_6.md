# Century5->6

## 概要

The password for Century6 is the short name of the domain in which this system resides in PLUS the name of the file on the desktop.

NOTE:

- If the short name of the domain is "blob" and the file on the desktop is named "1234", the password would be "blob1234".
- The password will be lowercase no matter how it appears on the screen.

## 解法

まずはデスクトップのファイル名を取得

```Powershell
> ls ..\Desktop


    Directory: C:\Users\century5\Desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018   3:29 AM             54 3347
```

ドメイン名を取得したい

`FT <表示列の選択>`: プロパティを表形式で表示．

```Powershell
> Get-ADDomain | FT NetBIOSName

NetBIOSName
-----------
underthewire
```

## FLAG

- `ssh century6@century.underthewire.tech`
- パスワード: underthewire3347