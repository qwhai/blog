```bat
signtool.exe sign /p "[password]" /f [证书目录] /tr "http://sha256timestamp.ws.symantec.com/sha256/timestamp" /fd sha256 /v [待签名软件目录]
```
or
```bat
signtool sign /p "[password]" /f [证书目录] /tr "http://sha256timestamp.ws.symantec.com/sha256/timestamp" /fd sha256 /v [待签名软件目录]
```
