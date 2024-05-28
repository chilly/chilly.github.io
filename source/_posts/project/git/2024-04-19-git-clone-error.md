---
title: 2024-04-19-git-clone-error
date: 2024-04-19 00:03:41
tags: [git,network]
category:
- [project, git]
---

执行：
```bash
git clone https://github.com/adieyal/sd-dynamic-prompts.git
```
出了问题，问题是:
```bash
Cloning into 'sd-dynamic-prompts'...
remote: Enumerating objects: 5626, done.
remote: Counting objects: 100% (600/600), done.
remote: Compressing objects: 100% (249/249), done.
error: RPC failed; curl 92 HTTP/2 stream 0 was not closed cleanly: CANCEL (err 8)
error: 906 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

我感觉可能是下载的东西有点多，网速较慢？改成`--depth 1`
```bash
git clone https://github.com/adieyal/sd-dynamic-prompts.git --depth 1
```

结果问题依旧，仍然是：
```bash
error: 1783 bytes of body are still expectediB | 193.00 KiB/s
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```
尝试多次，仍然是这个问题，感觉这个和网速也没有太大的问题。继续查资料。修改git配置
```bash
git config --global core.compression 0
```

报错为：
```bash
Cloning into 'sd-dynamic-prompts'...
error: RPC failed; curl 16 Error in the HTTP2 framing layer
fatal: expected flush after ref listing
```

继续修改：
```bash
git config --global http.version HTTP/1.1
git config --global http.postBuffer 157286400
```
这个发现改完后，直接卡住了。不下载了。我继续修改：
```bash
apt-get install gnutls-bin
git config --global http.sslVerify false
git config --global http.postBuffer 1048576000
git config --global http.version HTTP/2
```

这次终于成功了。但是这种`--depth 1`的下载方法是没有history的。如果希望有history commit.则进去到刚下载的resposity中。
然后执行
```bash
git pull --unshallow
```
这样就能把整个修改历史都pull下来了。