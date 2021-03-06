# FTP-Deployer

FTP-Deployer 是一個基於 FTP 的檔案傳輸工具，您可以透過簡單的設定快速傳輸兩個主機之間的資料。

## Installation

```
npm install -g ftp-deployer
```

## Pure CLI
```
dep <feature> <options>
```

## Examples

假設我們要將 `localhsot` 上的 `./myproject` 佈署到 `192.168.1.1` 主機上的 `/var/www`

> 兩台主機都必須安裝 `FTP-Deployer`

### Server-side
啟動 FTP Server
```
dep server -d /var/www 
```

> 請注意若沒有指定 `-p` 連接埠，預設會開啟 `21`，請注意防火牆是否能允許通過。

output:
```
FTP-Server listening on 127.0.0.1 port 21
Root Path at: /var/www
```

### Client-side
傳送目錄下的所有檔案到 Server
```
dep publish -d ./myproject -h 192.168.1.1
```

output:
```
[success] \index.php
[success] \server.php
end
close
```

## Options
更多設定可以參考如下

### Server-side

 option | short | description 
--- | --- | ---
 --host | -h | 指定 Server IP，預設為 `127.0.0.1` (localhost) 
 --port | -p | 指定 Server Port，預設為 `21` (FTP) 
 --dir | -d | 指定 FTP Server 的根目錄，預設為當前目錄 

#### Example
```
dep server -h 192.168.1.1 -p 888 -d /var/www/html
```

### Client-side


 option | short | description 
--- | --- | ---
 --host | -h | 指定欲傳送到的 IP，預設為 `127.0.0.1` (localhost) 
 --port | -p | 指定欲傳送到的 port，預設為 `21` (FTP) 
 --dir | -d | 指定目錄 (目錄下的所有檔案會被傳送)，預設為當前目錄 
 --ignore | -i | 指定欲忽略部傳送的檔案，預設不忽略任何檔案 (可以使用`*`來進行模糊匹配) 




#### Example
```
dep publish -h 192.168.1.1 -p 888 -d ./myproject -i .git,node_modules,setting.json
```

<br>

## Configuration File

你也可以透過添加設定檔來進行 option 設定，設定檔必須**存放在當前的執行目錄下**。格式如下所示：

### Server-side
設定檔案名稱必須為 **`server.json`**
```json
{
  "host": "127.0.0.1",
  "port": 21,
  "rootdir": "."
}
```

### Client-side
設定檔案名稱必須為 **`publish.json`**
```json
{
  "host": "192.168.1.1",
  "port": 21,
  "rootdir": ".",
  "ignore": [".git", "node_modules", "*.setting"]
}
```

> 若您撰寫了設定檔則不需要在執行加註 option，
> 但若也在 CLI 中設定 option 會覆蓋設定檔中的內容。
> 
> 設定檔中的項目皆為可選，若無設定會使用系統預設參數。

#### Example

```
dep publish -p 888
```
> 此時以 port 888 為主


## LICENSE

```
Copyright (C) 2016 ZapLin

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
of the Software, and to permit persons to whom the Software is furnished to do
so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
