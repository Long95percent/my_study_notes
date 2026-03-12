
准备：
下载BurpSuits Community


![[Pasted image 20251209101325.png]]
下载之后放在phpstudy的WWW文件夹下，并修改config的账号密码
接下来搭建靶场
![[Pasted image 20251209102850.png]]
打开phpstudy首页的数据库工具和Apache
进入127.0.0.1/dvwa
![[Pasted image 20251209103416.png]]
![[Pasted image 20251209103437.png]]出现问题
![[Pasted image 20251209103708.png]]
SQL服务没打开
用户登录：
![[Pasted image 20251209104107.png]]
登陆后把安全等级调为Low
![[Pasted image 20251209105137.png]]
点击CSFR下方的View Sourse查看原代码，发现没有任何防CSRF机制，可以用这个漏洞进行攻击
![[Pasted image 20251209105506.png]]
在网址栏输入参数：
在后面加上：
?password_new=password&password_conf=password&Change=Change#
![[Pasted image 20251209105937.png]]
Password changed.
当受害者点击该链接，TA的密码就被修改了
把难度调为Medium
![[Pasted image 20251209110154.png]]
再次看源码，可以看到加了一个关键检查：
![[Pasted image 20251209110642.png]]
接下来就要用BurpSuit增加reffer
打开代理：
![[Pasted image 20251209110922.png]]
输入新的账号密码：123456，打开Burp-Proxy-Intercept-on,然后点击change
![[Pasted image 20251209111112.png]]
![[Pasted image 20251209111126.png]]
什么都没抓到
![[Pasted image 20251209111710.png]]
检查代理配置，也正常
![[Pasted image 20251209115910.png]]
![[Pasted image 20251209120221.png]]
对其它网站有影响，但是对测试网站无法抓包
[
    {
        "name": "Burp Suite Navigation Recorder",
        "version": "2.0.13",
        "recordingDelay": 0,
        "eventType": "start",
        "platform": "Win32",
        "iframes": [],
        "windows": [
            {
                "windowId": 2018332639
            }
        ],
        "tabs": [
            {
                "tabId": 2018332640,
                "windowId": 2018332639,
                "attributes": {
                    "firstUrl": "http://127.0.0.1/dvwa/security.php"
                }
            }
        ]
    },
    {
        "date": "2025-12-09T04:24:21.505Z",
        "timestamp": 1765254261505,
        "eventType": "goto",
        "url": "http://127.0.0.1/dvwa/security.php",
        "triggersNavigation": true,
        "frameId": 0,
        "tabId": 2018332640,
        "windowId": 2018332639,
        "fromAddressBar": true
    },
    {
        "date": "2025-12-09T04:24:22.694Z",
        "timestamp": 1765254262694,
        "eventType": "scroll",
        "scrollX": 0,
        "scrollY": 218,
        "innerWidth": 1440,
        "innerHeight": 731,
        "url": "http://127.0.0.1/dvwa/security.php",
        "frameId": 0,
        "tabId": 2018332640,
        "windowId": 2018332639,
        "isIframe": false
    },
    {
        "date": "2025-12-09T04:24:24.550Z",
        "timestamp": 1765254264550,
        "windowInnerWidth": 1440,
        "windowInnerHeight": 731,
        "eventType": "click",
        "frameId": 0,
        "tabId": 2018332640,
        "triggersNavigation": true,
        "triggersWithinDocumentNavigation": false,
        "isShadowElement": false,
        "tagName": "INPUT",
        "placeholder": "",
        "className": "",
        "name": "seclev_submit",
        "id": "",
        "textContent": "",
        "innerHTML": "",
        "value": "Submit",
        "elementType": "submit",
        "textNodes": [],
        "tagNodeIndex": 0,
        "xPath": "/html/body/div/div[3]/div/form/input[1]",
        "characterPos": null,
        "shiftKey": false,
        "ctrlKey": false,
        "altKey": false,
        "metaKey": false,
        "url": "http://127.0.0.1/dvwa/security.php",
        "windowId": 2018332639,
        "isIframe": false
    },
    {
        "date": "2025-12-09T04:24:24.572Z",
        "timestamp": 1765254264572,
        "eventType": "userNavigate",
        "url": "http://127.0.0.1/dvwa/security.php#",
        "frameId": 0,
        "tabId": 2018332640,
        "windowId": 2018332639
    },
    {
        "date": "2025-12-09T04:24:26.051Z",
        "timestamp": 1765254266051,
        "windowInnerWidth": 1440,
        "windowInnerHeight": 731,
        "eventType": "click",
        "frameId": 0,
        "tabId": 2018332640,
        "triggersNavigation": true,
        "triggersWithinDocumentNavigation": false,
        "isShadowElement": false,
        "tagName": "A",
        "href": "vulnerabilities/csrf/",
        "className": "",
        "name": "",
        "id": "",
        "textContent": "CSRF",
        "innerHTML": "CSRF",
        "elementType": "",
        "textNodes": [
            "CSRF"
        ],
        "tagNodeIndex": 5,
        "xPath": "/html/body/div/div[2]/div/ul[2]/li[3]/a",
        "shiftKey": false,
        "ctrlKey": false,
        "altKey": false,
        "metaKey": false,
        "url": "http://127.0.0.1/dvwa/security.php",
        "windowId": 2018332639,
        "isIframe": false
    },
    {
        "date": "2025-12-09T04:24:26.062Z",
        "timestamp": 1765254266062,
        "eventType": "userNavigate",
        "url": "http://127.0.0.1/dvwa/vulnerabilities/csrf/",
        "frameId": 0,
        "tabId": 2018332640,
        "windowId": 2018332639
    },
    {
        "date": "2025-12-09T04:24:27.207Z",
        "timestamp": 1765254267207,
        "windowInnerWidth": 1440,
        "windowInnerHeight": 731,
        "eventType": "click",
        "frameId": 0,
        "tabId": 2018332640,
        "triggersNavigation": false,
        "triggersWithinDocumentNavigation": false,
        "isShadowElement": false,
        "tagName": "INPUT",
        "placeholder": "",
        "className": "",
        "name": "password_new",
        "id": "",
        "textContent": "",
        "innerHTML": "",
        "value": "",
        "elementType": "password",
        "textNodes": [],
        "tagNodeIndex": 0,
        "xPath": "/html/body/div/div[3]/div/div/form/input",
        "characterPos": 0,
        "shiftKey": false,
        "ctrlKey": false,
        "altKey": false,
        "metaKey": false,
        "url": "http://127.0.0.1/dvwa/vulnerabilities/csrf/",
        "windowId": 2018332639,
        "isIframe": false
    },
    {
        "date": "2025-12-09T04:24:28.274Z",
        "timestamp": 1765254268274,
        "windowInnerWidth": 1440,
        "windowInnerHeight": 731,
        "eventType": "typing",
        "frameId": 0,
        "tabId": 2018332640,
        "triggersNavigation": false,
        "triggersWithinDocumentNavigation": false,
        "isShadowElement": false,
        "tagName": "INPUT",
        "placeholder": "",
        "className": "",
        "name": "password_new",
        "id": "",
        "textContent": "",
        "innerHTML": "",
        "value": "",
        "elementType": "password",
        "textNodes": [],
        "tagNodeIndex": 0,
        "xPath": "/html/body/div/div[3]/div/div/form/input",
        "typedValue": "123456",
        "url": "http://127.0.0.1/dvwa/vulnerabilities/csrf/",
        "windowId": 2018332639,
        "isIframe": false
    },
    {
        "date": "2025-12-09T04:24:30.830Z",
        "timestamp": 1765254270830,
        "windowInnerWidth": 1440,
        "windowInnerHeight": 731,
        "eventType": "click",
        "frameId": 0,
        "tabId": 2018332640,
        "triggersNavigation": false,
        "triggersWithinDocumentNavigation": false,
        "isShadowElement": false,
        "tagName": "INPUT",
        "placeholder": "",
        "className": "",
        "name": "password_conf",
        "id": "",
        "textContent": "",
        "innerHTML": "",
        "value": "",
        "elementType": "password",
        "textNodes": [],
        "tagNodeIndex": 1,
        "xPath": "/html/body/div/div[3]/div/div/form/input[2]",
        "characterPos": 0,
        "shiftKey": false,
        "ctrlKey": false,
        "altKey": false,
        "metaKey": false,
        "url": "http://127.0.0.1/dvwa/vulnerabilities/csrf/",
        "windowId": 2018332639,
        "isIframe": false
    },
    {
        "date": "2025-12-09T04:24:31.549Z",
        "timestamp": 1765254271549,
        "windowInnerWidth": 1440,
        "windowInnerHeight": 731,
        "eventType": "typing",
        "frameId": 0,
        "tabId": 2018332640,
        "triggersNavigation": false,
        "triggersWithinDocumentNavigation": false,
        "isShadowElement": false,
        "tagName": "INPUT",
        "placeholder": "",
        "className": "",
        "name": "password_conf",
        "id": "",
        "textContent": "",
        "innerHTML": "",
        "value": "",
        "elementType": "password",
        "textNodes": [],
        "tagNodeIndex": 1,
        "xPath": "/html/body/div/div[3]/div/div/form/input[2]",
        "typedValue": "123456",
        "url": "http://127.0.0.1/dvwa/vulnerabilities/csrf/",
        "windowId": 2018332639,
        "isIframe": false
    },
    {
        "date": "2025-12-09T04:24:34.130Z",
        "timestamp": 1765254274130,
        "windowInnerWidth": 1440,
        "windowInnerHeight": 731,
        "eventType": "click",
        "frameId": 0,
        "tabId": 2018332640,
        "triggersNavigation": true,
        "triggersWithinDocumentNavigation": false,
        "isShadowElement": false,
        "tagName": "INPUT",
        "placeholder": "",
        "className": "",
        "name": "Change",
        "id": "",
        "textContent": "",
        "innerHTML": "",
        "value": "Change",
        "elementType": "submit",
        "textNodes": [],
        "tagNodeIndex": 2,
        "xPath": "/html/body/div/div[3]/div/div/form/input[3]",
        "characterPos": null,
        "shiftKey": false,
        "ctrlKey": false,
        "altKey": false,
        "metaKey": false,
        "url": "http://127.0.0.1/dvwa/vulnerabilities/csrf/",
        "windowId": 2018332639,
        "isIframe": false
    },
    {
        "date": "2025-12-09T04:24:34.151Z",
        "timestamp": 1765254274151,
        "eventType": "userNavigate",
        "url": "http://127.0.0.1/dvwa/vulnerabilities/csrf/?password_new=123456&password_conf=123456&Change=Change#",
        "frameId": 0,
        "tabId": 2018332640,
        "windowId": 2018332639
    }
]