# Century9->10

The password for Century10 is the 161st word within the file on the desktop. 

NOTE:

- The password will be lowercase no matter how it appears on the screen. 

## 解法

- $arrayに文字列をコピーしよう
  - `> $array = cat .\Word_File.txt`
- " "で分割してみよう
  - `> $array.split(" ")`
- 指定した番号の文字を表示する
  - `> $array.split(" ")[160]`

これ161番目ってことは0から数えて160なんさな

```Powershell
> $array.split(" ")[160]
pierid
```

## FLAG

- `ssh century10@century.underthewire.tech`
- pierid