# Century0->1

## 概要

The goal of this level is to log into the game. In order to access the game, you will need to obtain the initial credentials via the #StartHere channel on our Slack (https://underthewire.herokuapp.com/). After obtaining the credentials, choose one of the two options below to connect to the server. 

We recommend using SSH as PSRemoting is in the experimental state. No functionality is lost with SSH as you will be dropped into a PowerShell console. 

### Option One - SSH (Preferred Method) 

1. You will need a SSH client such as Putty. The host to which you need to connect is century.underthewire.tech, on port 22. 

2. When prompted, use the credentials for the applicable game find in the #StartHere Slack channel. 

3. You have successfully connected to the game server when your path changes to "[century.underthewire.tech]:" 

### Option Two - PSRemoting 

Known Issues:

- Some users report connection errors when attempting to enter a PSSession.
- Some users may experience a "max user" error when going from level to level. This is due to a limitation from Microsoft with the number of concurrent users using PSRemoting. The error usually resolves itself within a few minutes. 

You will need to import the server's certificate and start a remote interactive session with the game server. 

1. Open PowerShell on your system. 

2. Copy and paste the below in order to import the certificate for the game server. Without this, you will not be able to connect to it.

```bash
$webRequest = [Net.WebRequest]::Create("https://games.underthewire.tech:5986/wsman")
try { $webRequest.GetResponse() } catch {}
$cert = $webRequest.ServicePoint.Certificate
$store = New-Object System.Security.Cryptography.X509Certificates.X509Store -ArgumentList  "Root", "CurrentUser"
$store.Open('ReadWrite')
$store.Add($cert)
$store.Close()
Test-WSMan -ComputerName games.underthewire.tech -port 5986 -UseSSL
```

1. Copy and paste the below to initiate the connection to the game server. You will need to do this for each level you log into as well.

```bash
Enter-PSSession -ComputerName games.underthewire.tech -UseSSL -port 5986 -Credential (Get-Credential)
```

4. When prompted, use the credentials for the applicable game find in the #StartHere Slack channel. 

5. You have successfully connected to the game server when your path changes to "[games.underthewire.tech]: PS C:\Users\century1\Documents>"

## 解法

そのままやるだけ．