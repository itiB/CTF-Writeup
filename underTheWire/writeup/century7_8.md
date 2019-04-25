# Century7->8

## 概要

The password for Century8 is in a readme file somewhere within the contacts, desktop, documents, downloads, favorites, music, or videos folder in the user's profile.

NOTE:

- The password will be lowercase no matter how it appears on the screen.

## 解法

Get-ChildItemに検索条件を指定．fullnameを選択することでディレクトリまで教えてもらうことができる

```Powershell
> Get-ChildItem "*/*readme*", "*readme*" | select fullname

FullName
--------
C:\Users\century7\Downloads\Readme


PS C:\Users\century7> cat .\Downloads\Readme
7points
```

## FLAG

- `ssh century8@century.underthewire.tech`
- 7points