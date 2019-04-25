# Century8->9

## 概要

The password for Century9 is the number of unique entries within the file on the desktop.

## 解法

uniqueの中身をGroupでまとめてそれぞれのグループの数を数えてみよう

```Powershell
> cat .\unique.txt | Group | Format-Table count,name

Count Name
----- ----
    1 recreatively
    1 proboscidean
    1 behear
    1 iniquitable
~~~~~
    1 ipseity
    1 morcellate
    1 underbodice
    1 noneugenically
    1 appetit
    1 transbaikalian
```

全部ユニークじゃん！！！！！
普通に数えちゃえばいいじゃん

```Powershell
> cat .\unique.txt | Measure-Object


Count    : 696
Average  :
Sum      :
Maximum  :
Minimum  :
Property :
```

## FLAG

- `ssh century9@century.underthewire.tech`
- 696