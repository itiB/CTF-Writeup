# Century3->4

## 概要

The password for Century4 is the number of files on the desktop. 

## 解法

ディレクトリ内のフォルダ一覧を表示: `Get-ChildItem`  
オブジェクトの数を表示: `Measure-Object`

```powershell
> Get-ChildItem | Measure-Object


Count    : 123
Average  :
Sum      :
Maximum  :
Minimum  :
Property :
```

## FLAG

- `ssh century4@century.underthewire.tech`  
- 123