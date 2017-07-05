---
layout: post
title: "UDP Flood after thought"
status: draft
type: post
comments: true
list: general
set: general
---

We just have been atacked by chineese flooders. The battle took several days and our VPS was blocked few times. Situations like these taught me value of properly configured Firewall software.

<!--more-->

More than a week ago I woke up and saw this beautiful message.

```

Attack detail : 163Kpps/55Mbps
dateTime                   srcIp:srcPort           dstIp:dstPort           protocol flags       bytes reason
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            42 ATTACK:UDP
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            41 ATTACK:UDP
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            48 ATTACK:UDP
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            48 ATTACK:UDP
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            46 ATTACK:UDP
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            46 ATTACK:UDP
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            42 ATTACK:UDP
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            48 ATTACK:UDP
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            44 ATTACK:UDP
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            40 ATTACK:UDP
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            40 ATTACK:UDP
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            47 ATTACK:UDP
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            48 ATTACK:UDP
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            46 ATTACK:UDP
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            44 ATTACK:UDP
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            44 ATTACK:UDP
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            49 ATTACK:UDP
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            42 ATTACK:UDP
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            46 ATTACK:UDP
2017.06.22 06:59:17 CEST   51.254.32.54:38481      60.191.143.14:7003      UDP      ---            49 ATTACK:UDP
```


I instantaneously crazy went bananas. After brief discussion with hosting representatives I managed to get emergency SSH access. First thing I thought about checking syslog and authlog. Authlog was litteraly filled with failed login attempts. There was far to many of them to comprehend anything so I decided to make a quick tool for analysing those logs. That's how <a href="https://github.com/Hajto/who_ddosed_me"> who_ddosed_me </a> was born. After analysing first file I could clearly see that someone is scanning ports of my server. We've got like 11k ssh login requests to different sockets from a single IP (which was from China).

{% highlight json %}

{
    "ip": "59.63.166.81",
    "extras": [
      {
        "time": "Jun 21 23:43:39",
        "port": "56635"
      },
      {
        "time": "Jun 21 23:43:29",
        "port": "30563"
      },
      {
        "time": "Jun 21 23:43:11",
        "port": "2996"
      },
    ]
    "count": 11063
}

{% endhighlight  %}

After presenting evidence to hosting I've got my normal access back. I changed up all of the passwords and called it a day. Which was actually a big mistake, because few days later same situation has occured.

```
Your vps server has been blocked. Actions not permitted by rules were detected.

- LOGS START -

Attack detail : 117Kpps/25Mbps
dateTime srcIp:srcPort dstIp:dstPort protocol flags
bytes reason
2017.06.30 18:01:56 CEST 51.254.32.54:40376 162.218.55.250:80 UDP ---
1010 ATTACK:UDP
2017.06.30 18:01:56 CEST 51.254.32.54:40376 162.218.55.250:80 UDP ---
1010 ATTACK:UDP
2017.06.30 18:01:56 CEST 51.254.32.54:40376 162.218.55.250:80 UDP ---
1010 ATTACK:UDP
2017.06.30 18:01:56 CEST 51.254.32.54:40376 162.218.55.250:80 UDP ---
1010 ATTACK:UDP
2017.06.30 18:01:56 CEST 51.254.32.54:40376 162.218.55.250:80 UDP ---
1010 ATTACK:UDP
2017.06.30 18:01:56 CEST 51.254.32.54:40376 162.218.55.250:80 UDP ---
1010 ATTACK:UDP
```

At that point I've had enough. I decided to take additional security measures. First of all I checked why Firewall wasn't working. For some reason it appeared to be offline... Ok.. The quickest and safest solution would make new, fresh config. I've also decided to not use Login/Password credentials for SSH and use public/private key pairs for authentication only. I've also disabled login for root.

After those actions amount of attempts logged in authlog severaly dropped. It has been few days since I've done this and it seems to solve the problem. I learned my lesson the hard way. I suggest you should do the same, before you are going to be victim of such vile actions.
