# oneliner-cmds
Useful one liner commands for data parsing

### Test input:
```
┌──(natraj㉿natraj-kali)-[~]
└─$ ifconfig eth0                                                     
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.195.128  netmask 255.255.255.0  broadcast 192.168.195.255
        inet6 fe80::20c:29ff:fe34:5c1c  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:34:5c:1c  txqueuelen 1000  (Ethernet)
        RX packets 124  bytes 8243 (8.0 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 34  bytes 3927 (3.8 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
### To extract IP address
```
┌──(natraj㉿natraj-kali)-[~]
└─$ ifconfig eth0 | grep -Po "(?i)\binet (\d+\.){3}\d+\b" | cut -d" " -f2
192.168.195.128
```
### To extract netmask (similar to above)
```
┌──(natraj㉿natraj-kali)-[~]
└─$ ifconfig eth0 | grep -Po "(?i)\bnetmak (\d+\.){3}\d+\b" | cut -d" " -f2
255.255.255.0
```
### To extract mac address
```
┌──(natraj㉿natraj-kali)-[~]
└─$ ifconfig eth0 | grep -Po "(?i)\bether ([\da-f]{2}:){5}[\da-f]{2}\b" | cut -d" " -f2
00:0c:29:34:5c:1c
```
### To extract IPv6 address
```
┌──(natraj㉿natraj-kali)-[~]
└─$ ifconfig eth0 | grep -Po "(?i)\binet6 [\da-f:]+\b" | cut -d" " -f2
fe80::20c:29ff:fe34:5c1c
```
### Simple oneliner to fetch all unique URLs from html file.
```
┌──(natraj㉿natraj-kali)-[~/Desktop]
└─$ cat test.html | grep -Po "https?://[^\");]+" | sort -u
https://api.search.yahoo.com/data/v3/search?appid=4d234a9d&market=
https://cc.bingj.com/cache.aspx?q=Latest+Indian+news&amp
https://consent.cmp.oath.com/cmp.js
https://geo.yahoo.com/t
https://guce.yahoo.com/privacy-dashboard?locale=en-IN&amp
https://help.yahoo.com/kb/search-for-desktop/yahoo-search-suggestions-sln26943.html
https://in.help.yahoo.com/kb/search-for-desktop
https://in.help.yahoo.com/kb/search-for-desktop/yahoo-search-suggestions-sln26943.html
https://in.images.search.yahoo.com/search/images
https://in.mail.yahoo.com/?.intl=in&amp
...
...
```
### Simple one liner to fetch unique domain names from html file.
```
┌──(natraj㉿natraj-kali)-[~/Desktop]
└─$ cat test.html | grep -Po "https?://[^/\"]+" | sort -u
https://api.search.yahoo.com
https://cc.bingj.com
https://consent.cmp.oath.com
https://geo.yahoo.com
https://guce.yahoo.com
https://help.yahoo.com
https://in.help.yahoo.com
https://in.images.search.yahoo.com
https://in.mail.yahoo.com
...
...
```
### Simple oneliner to count each domain name called from the html file. 
```
┌──(natraj㉿natraj-kali)-[~/Desktop]
└─$ cat test.html | grep -Po "https?://[^/\"]+" | sort | uniq -c | sort -r
     41 https://in.search.yahoo.com
     30 https://s.yimg.com
      7 https://timesofindia.indiatimes.com
      6 https://cc.bingj.com
      3 https://www.myfonts.com
      3 https://in.news.search.yahoo.com
      3 https://in.help.yahoo.com
      2 https://sports.ndtv.com
      2 https://legal.yahoo.com
      2 https://geo.yahoo.com
      1 https://www.ndtv.com
      1 https://www.indiatoday.in
      1 https://login.yahoo.com
      1 https://indianexpress.com
      1 https://in.video.search.yahoo.com
      1 https://in.mail.yahoo.com
...
...
```
### To count HTML tags count from given html file.
```
┌──(natraj㉿natraj-kali)-[~/Desktop]
└─$ cat test.html | grep -Po "<\w+>" | sort | uniq -c | sort -r 
     86 <b>
     25 <span>
     15 <tr>
     14 <li>
     13 <strong>
      6 <div>
      4 <style>
      3 <p>
      2 <ul>
      2 <tbody>
      2 <script>
      1 <title>
      1 <noscript>
      1 <ins>
      1 <head>
      1 <form>
      1 <footer>
      1 <button>
```
### Collect and append a string("test") to all collected domain names.
```
┌──(natraj㉿natraj-kali)-[~/Desktop]
└─$ cat test.html | grep -Po "https?://[^/\"]+" | sort -u | sed -re 's#(https://)#\1test.#'g                  
https://test.api.search.yahoo.com
https://test.cc.bingj.com
https://test.consent.cmp.oath.com
https://test.geo.yahoo.com
https://test.guce.yahoo.com
https://test.help.yahoo.com
https://test.in.help.yahoo.com
https://test.in.images.search.yahoo.com
https://test.in.mail.yahoo.com
https://test.in.news.search.yahoo.com
https://test.in.search.yahoo.com
https://test.in.video.search.yahoo.com
https://test.indianexpress.com
https://test.legal.yahoo.com
https://test.login.yahoo.com
https://test.s.yimg.com
https://test.sports.ndtv.com
https://test.timesofindia.indiatimes.com
https://test.www.indiatoday.in
https://test.www.myfonts.com
https://test.www.ndtv.com
```
### To remove unwanted strings/data from output.(removed "www." from the above output)
```
┌──(natraj㉿natraj-kali)-[~/Desktop]
└─$ cat test.html | grep -Po "https?://[^/\"]+" | sort -u | sed -re 's#(https://)#\1test.#'g -e 's#www.##g'   
https://test.api.search.yahoo.com
https://test.cc.bingj.com
https://test.consent.cmp.oath.com
https://test.geo.yahoo.com
https://test.guce.yahoo.com
https://test.help.yahoo.com
https://test.in.help.yahoo.com
https://test.in.images.search.yahoo.com
https://test.in.mail.yahoo.com
https://test.in.news.search.yahoo.com
https://test.in.search.yahoo.com
https://test.in.video.search.yahoo.com
https://test.indianexpress.com
https://test.legal.yahoo.com
https://test.login.yahoo.com
https://test.s.yimg.com
https://test.sports.ndtv.com
https://test.timesofindia.indiatimes.com
https://test.indiatoday.in
https://test.myfonts.com
https://test.ndtv.com
```                    
