# Century6->7

## 概要

The password for Century7 is the number of folders on the desktop.

## 解法

これどっかでみたな？？？？

```Powershell
> Get-ChildItem | Measure-Object


Count    : 197
Average  :
Sum      :
Maximum  :
Minimum  :
Property :
```

## FLAG

- `ssh century7@century.underthewire.tech`
- 197