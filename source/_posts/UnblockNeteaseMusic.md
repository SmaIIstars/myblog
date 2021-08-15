---
title: UnblockNeteaseMusic
tags: Welfare
abbrlink: 306a286d
date: 2021-02-12 14:23:31
---

# UnblockNeteaseMusic

**Unlock netease Cloud Music client greying songs.**

## Install

```git
git clone https://gitclone.com/github.com/ITJesse/UnblockNeteaseMusic.git
```

|      Abbreviation      |      Command      |                   Note                    |
| :--------------------: | :---------------: | :---------------------------------------: |
|           -v           |     --version     |         output the version number         |
|        -p port         |    --port port    |            specify server port            |
|       -a address       | --address address |            specify server host            |
|         -u url         |  --proxy-url url  |      request through upstream proxy       |
|        -f host         | --force-host host |        force the netease server ip        |
| -o source [source ...] |   --match-order   |            source [source ...]            |
|        -t token        |   --token token   |        set up proxy authentication        |
|         -e url         |  --endpoint url   | replace virtual endpoint with public host |
|           -s           |     --strict      |          enable proxy limitation          |
|           -h           |      --help       |         output usage information          |

```node
node app -p port
```

## Usage

- PC

  1. Open the PC version of netease cloud, click the Settings button on the right of avatar, and navigate to the Tools section.
  2. Select the custom proxy, the HTTP proxy.
  3. Input the proxy service and port.
  4. Test success can be used.

  ![PC](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/UnblockNeteaseMusic-PC.png)

- Mobile & Tablet

  - Built-in network agent: New environment needs to be reconfigured, but no third-party agent software is required

    1. WIFI

       ![](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/UnblockNeteaseMusic-Mobile&Tablet1.png)

       ![](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/UnblockNeteaseMusic-Mobile&Tablet2.png)

    2. Mobile Network

       ![](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/UnblockNeteaseMusic-Mobile&Tablet3.png)

       ![](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/UnblockNeteaseMusic-Mobile&Tablet4.png)

       ![](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/UnblockNeteaseMusic-Mobile&Tablet5.png)

       ![](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/UnblockNeteaseMusic-Mobile&Tablet6.png)

  - Third-party agent software: Recommend Clash, Shadowrocket, Kitsunebi

## References

[Github](https://github.com/nondanee/UnblockNeteaseMusic)