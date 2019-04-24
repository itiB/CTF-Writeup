# Century1->2

## 概要

The password for Century2 is the build version of the instance of PowerShell installed on this system.

NOTE:

- The format is as follows: `**.*.*****.****`
- Include all periods

Once you feel you have completed the Century1 challenge, start a new connection to the server, and log in with the username of Century2 and this password will be the answer from Century1. If successful, close out the Century1 connection and begin to solve the Century2 challenge. This concept is repeated over and over until you reach the end of the game.

## 解法

Powershellのビルドバージョンを見るコマンドを打つ

```bash
> $PSVersionTable
Name Value
---- -----
PSVersion 5.1.14393.2791
PSEdition Desktop
PSCompatibleVersions {1.0, 2.0, 3.0, 4.0...}
BuildVersion 10.0.14393.2791
CLRVersion 4.0.30319.42000
WSManStackVersion 3.0
PSRemotingProtocolVersion 2.3
SerializationVersion 1.1.0.1
```

## FLAG

- `ssh century2@century.underthewire.tech`
- 10.0.14393.2791