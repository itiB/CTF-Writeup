# Century2->3

## 概要

The password for Century3 is the name of the built-in cmdlet that performs the wget like function within PowerShell PLUS the name of the file on the desktop.

NOTE:

- If the name of the cmdlet is "get-web" and the file on the desktop is named "1234", the password would be "get-web1234".
- The password will be lowercase no matter how it appears on the screen.

## 解法

エイリアスを見るコマンドを使ってみる

```powershell
> get-alias wget

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           wget -> Invoke-WebRequest
```

Desktopのファイルを見る

```powershell
> ls


    Directory: C:\Users\century2\Desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018   3:29 AM            693 443

```

## FLAG

- `ssh century3@century.underthewire.tech`
- invoke-webrequest443