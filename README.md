# LINE-BOT
實作創建一個LINE BOT機器人,並能使用一些簡易功能:

1.回報時間+IP位址

2.推播文字

3.推播地址

-----------------------------------------------------------------------
## LINE BOT 申請
網站: https://developers.line.biz/en/

新增一個 provider (服務機構，例如：XX 公司) 

→ Messaging API channel　

→ 掃描QR code可加入好友

## 程式碼部分
### LINUX - Ubuntu-18.04.6-live-server-amd64
安裝LINE BOT及相關套件
```
$ sudo apt install python3-pip
$ pip3 install line-bot-sdk
$ pip3 install netifaces  #抓IP位址用
```

### PYTHON執行檔
```
# 匯入相關套件
import netifaces as ni
from datetime import datetime as T
from linebot import LineBotApi
from linebot.models import TextSendMessage,LocationSendMessage

# 編輯回報IP位址程式碼
etho = ni.interfaces()
ni.ifaddresses(etho[1])
ip = ni.ifaddresses(etho[1])[ni.AF_INET][0]['addr']

# 日誌系統
T = T.now().strftime("%Y-%m-%d %H:%M:%S")
message1 = (T,"program started.")
message2 = (T,"get IP address:",ip)
print (message1)
print (message2)

# 設定Broadcast的訊息內容:時間+IP位址
line_bot_api = LineBotApi(CHANNEL_ACCESS_TOKEN)  # TOKEN可在頻道中的"Messaging API"找到
line_bot_api.broadcast(TextSendMessage(text = str(message1)))
line_bot_api.broadcast(TextSendMessage(text = str(message2)))

# 設定Broadcast的訊息內容:文字及地址
line_bot_api.broadcast(TextSendMessage(text = "歡迎來到肉肉團購網!"))
line_bot_api.broadcast(LocationSendMessage(
    title = "肉肉團購總部",
    address = "台北市大安區建國南路二段231號",
    latitude = "25.026269015089266",
    longitude = "121.53809221221155"))
```

## 執行
```
$ python3 main.py
```
查看LINE即可看到結果
