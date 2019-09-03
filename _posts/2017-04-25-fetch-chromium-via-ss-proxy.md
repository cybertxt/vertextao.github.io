---

layout: post
title: "通过代理拉取chromium过程"
date: 2017-04-25 17:00:00 +0800

---

# windows上通过代理拉取chromium过程

## 前提

* 已经有工作正常的shadowsocks代理。关于shadowsocks的文章很多，这里就不重复了。

* shadowsocks 本地代理端口 ```1080```

* 以下所有命令都在 windows cmd 中执行（除非特殊说明）

## 下载安装 ```depot_tools```

参考 [Install depot_tools](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md#Install)

**注意：** 设置该环境变量 ```DEPOT_TOOLS_WIN_TOOLCHAIN``` 并重启 cmd shell 生效

## 拉取代码

### windows代理设置
```bat

# for general command line proxy
netsh winhttp set proxy 127.0.0.1:1080

# reset proxy when not needed
netsh winhttp reset proxy

# for python environment variables
set http_proxy=http://127.0.0.1:1080
set https_proxy=https://127.0.0.1:1080
set socks5_proxy=socks5://127.0.0.1:1080

# for git
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'

# unset git proxy when not needed
git config --global --unset http.proxy
git config --global --unset https.proxy

```

参考 [Get the code](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md#Get-the-code)

### linux 代理设置
```shell
#!/bin/bash 
setenv()
{
    export PATH=/home/caesar/depot_tools:$PATH
    export http_proxy="http://127.0.0.1:1080"
    export https_proxy="https://127.0.0.1:1080"
    export socks5_proxy="socks5://127.0.0.1:1080"
    git config --global http.proxy http://127.0.0.1:1080
    git config --global https.proxy https://127.0.0.1:1080
}

unsetenv()
{
    unset http_proxy
    unset https_proxy
    unset socks5_proxy
    git config --global --unset http.proxy
    git config --global --unset https.proxy
}
```

#### gsutil proxy

* file `gsutil_proxy.boto`

```conf
[Boto]
proxy = 192.168.26.234
proxy_port = 1080
```

* environment variable

```shell
export NO_AUTH_BOTO_CONFIG="/path/to/gsutil_proxy.boto"
```

## ATTENTION!

**`gclient sync` MUST be executed after switching to different branch.**

*refer to: [https://bugs.chromium.org/p/webrtc/issues/detail?id=7762#c9](https://bugs.chromium.org/p/webrtc/issues/detail?id=7762#c9)*

## Issue about the win10 sdk version problem

* old windows sdk versions:
    + [https://developer.microsoft.com/en-us/windows/downloads/sdk-archive](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive)

*refer to [https://groups.google.com/a/chromium.org/d/msg/chromium-dev/iVZ8imodzh4/iGbSjhr5CgAJ](https://groups.google.com/a/chromium.org/d/msg/chromium-dev/iVZ8imodzh4/iGbSjhr5CgAJ)*