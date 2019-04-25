# Century10->11

## 概要

The password for Century11 is the 10th and 8th word of the Windows Update service description combined PLUS the name of the file on the desktop.

NOTE:

- The password will be lowercase no matter how it appears on the screen.
- If the 10th and 8th word of the service description is "apple" and "juice" and the name of the file on the desktop is "88", the password would be "applejuice88".

## 解法

Windows Updateに関するサービス情報を手に入れるためにサービスの名前が知りたい

```Powershell
> Get-service -displayname "Windows Update"

Status   Name               DisplayName
------   ----               -----------
Stopped  wuauserv           Windows Update
```

真名がわかったところで，検索です

```Powershell
> get-service -name "wuauserv" | Select-Object *

Name                : wuauserv
RequiredServices    : {rpcss}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : False
DisplayName         : Windows Update
DependentServices   : {}
MachineName         : .
ServiceName         : wuauserv
ServicesDependedOn  : {rpcss}
ServiceHandle       :
Status              : Stopped
ServiceType         : Win32ShareProcess
StartType           : Manual
Site                :
Container           :
```

なんか足りない

```Powershell
> Get-WMIObject -Class Win32_Service -Filter "Name='wuauserv' "


ExitCode  : 0
Name      : wuauserv
ProcessId : 0
StartMode : Manual
State     : Stopped
Status    : OK
```

これで全部出してもらおう

```Powershell
> Get-WMIObject -Class Win32_Service -Filter "Name='wuauserv' " | Select *


PSComputerName          : UTW
Name                    : wuauserv
Status                  : OK
ExitCode                : 0
DesktopInteract         : False
ErrorControl            : Normal
PathName                : C:\Windows\system32\svchost.exe -k netsvcs
ServiceType             : Share Process
StartMode               : Manual
__GENUS                 : 2
__CLASS                 : Win32_Service
__SUPERCLASS            : Win32_BaseService
__DYNASTY               : CIM_ManagedSystemElement
__RELPATH               : Win32_Service.Name="wuauserv"
__PROPERTY_COUNT        : 26
__DERIVATION            : {Win32_BaseService, CIM_Service, CIM_LogicalElement,
                          CIM_ManagedSystemElement}
__SERVER                : UTW
__NAMESPACE             : root\cimv2
__PATH                  : \\UTW\root\cimv2:Win32_Service.Name="wuauserv"
AcceptPause             : False
AcceptStop              : False
Caption                 : Windows Update
CheckPoint              : 0
CreationClassName       : Win32_Service
DelayedAutoStart        : False
Description             : Enables the detection, download, and installation of
                          updates for Windows and other programs. If this service
                          is disabled, users of this computer will not be able to
                          use Windows Update or its automatic updating feature, and
                          programs will not be able to use the Windows Update Agent
                          (WUA) API.
DisplayName             : Windows Update
InstallDate             :
ProcessId               : 0
ServiceSpecificExitCode : 0
Started                 : False
StartName               : LocalSystem
State                   : Stopped
SystemCreationClassName : Win32_ComputerSystem
SystemName              : UTW
TagId                   : 0
WaitHint                : 0
Scope                   : System.Management.ManagementScope
Path                    : \\UTW\root\cimv2:Win32_Service.Name="wuauserv"
Options                 : System.Management.ObjectGetOptions
ClassPath               : \\UTW\root\cimv2:Win32_Service
Properties              : {AcceptPause, AcceptStop, Caption, CheckPoint...}
SystemProperties        : {__GENUS, __CLASS, __SUPERCLASS, __DYNASTY...}
Qualifiers              : {dynamic, Locale, provider, UUID}
Site                    :
Container               :
```

説明なるものがあった！これだけ取り出すときは...  
Select-object: プロパティの値を指定してを出力するコマンド

```Powershell
> Get-WMIObject -Class Win32_Service -Filter  "Name='wuauserv'"  | Select-Object -ExpandProperty Description
Enables the detection, download, and installation of updates for Windows and other programs. If this service is disabled, users of this computer will not be able to use Windows Update or its automatic updating feature, and programs will not be able to use the Windows Update Agent (WUA) API.
```

これの指定された10番目と8番目の文字をつなげて...  
デスクトップのフォルダ名はいつも通りに取り出して

## FLAG

- `ssh century11@century.underthewire.tech`
- windowsupdates110
