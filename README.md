<p align="center">
  <img width="550" src="nali-cli.svg">
</p>

<h1 align="center">Nali IP CLI</h1>

<p align="center">:anchor: Parse info of IP Address or CDN's CNAME online without leaving your terminal. 一个支持在线查询 IP 归属地信息的命令行工具。</p>

<p align="center">
<a href="https://github.com/fantasyroot"><img alt="Author" src="https://img.shields.io/badge/Author-Anto17-blue.svg?style=flat-square"/></a>
<a href="https://www.npmjs.com/package/nali-ip-cli"><img alt="Version" src="https://img.shields.io/npm/v/nali-ip-cli.svg?style=flat-square"/></a>
<img slt="Download times" src="https://img.shields.io/npm/dt/nali-ip-cli?style=flat-square"/>
<img alt="License" src="https://img.shields.io/npm/l/nali-ip-cli.svg?style=flat-square"/>
</p>

## Feature
- [x] Query IP Address or CDN's CNAME online.
- [x] Cache for duplicate IP information.
- [x] Allows to select query server from multiple servers, and remember.
- [x] Used with other commands, receives standard output as a pipe, convert the IP address in it.
- [ ] Add other query server and schemas by user.
- [ ] Automatically switch query server when current is unavailable.

## Installation or Upgrade

```bash
yarn global add nali-ip-cli

# or

# npm install nali-ip-cli -g

## Optional: Add following lines to ~/.zshrc if you want alias built-in commands. Use raw ping by this way: \ping 223.5.5.5
# if (( $+commands[nali] )); then
#   alias ping="nali-ping"
#   alias dig="nali-dig"
#   alias traceroute="nali-traceroute"
#   alias tracepath="nali-tracepath"
#   alias nslookup="nali-nslookup"
# fi
```

> Prebuilt binaries is also available under the [`bin`](https://github.com/fantasyroot/nali-ip-cli/tree/master/bin) directory of the GitHub Repo.

## Usage

Query a simple IP address:

```bash
$ nali 1.145.1.4

1.145.1.4 [澳大利亚，新南威尔士州，悉尼，澳大利亚电信]
```

Query IP addresses:

```bash
$ nali 114.5.1.4 223.5.5.5

114.5.1.4 [印度尼西亚，雅加达，Indosat] 223.5.5.5 [中国，浙江，杭州，阿里云]
```

Ping domain query IP addresses

```bash
$ nali-ping github.com
# or
# ping github.com (with alias ping="nali-ping")

PING github.com (20.205.243.166 [新加坡，微软]): 56 data bytes
64 bytes from 20.205.243.166 [新加坡，微软]: icmp_seq=0 ttl=112 time=89.489 ms
64 bytes from 20.205.243.166 [新加坡，微软]: icmp_seq=1 ttl=112 time=89.627 ms
```

Query and parse IP addresses, CNAME from `stdin`:

```bash
$ dig www.baidu.com +short | nali

www.a.shifen.com. [百度旗下业务地域负载均衡系统]
180.101.50.242 [中国，江苏，南京，电信]
180.101.50.188 [中国，江苏，南京，电信]


$ nslookup www.gov.cn 223.5.5.5 | nali
Server:		223.5.5.5 [中国，浙江，杭州，阿里云]
Address:	223.5.5.5 [中国，浙江，杭州，阿里云]#53

Non-authoritative answer:
www.gov.cn	canonical name = www.gov.cn.bsgslb.cn. [白山云 CDN]
www.gov.cn.bsgslb.cn	canonical name = zgovweb.v.bsgslb.cn. [白山云 CDN]
Name:	zgovweb.v.bsgslb.cn [白山云 CDN]
Address: 183.134.34.28 [中国，浙江，嘉兴，电信]
Name:	zgovweb.v.bsgslb.cn [白山云 CDN]
Address: 183.134.34.26 [中国，浙江，嘉兴，电信]
Name:	zgovweb.v.bsgslb.cn [白山云 CDN]
Address: 144.123.124.25 [中国，山东，德州，电信]
```

Use Nali CLI built-in tools:

```bash
$ nali-nslookup blog.skk.moe

Server:         1.0.0.1 [美国 APNIC&CloudFlare 公共 DNS 服务器]
Address:        1.0.0.1 [美国 APNIC&CloudFlare 公共 DNS 服务器]#53

Non-authoritative answer:
Name:   blog.skk.moe
Address: 104.18.101.28 [美国 CloudFlare 公司 CDN 节点]
Name:   blog.skk.moe
Address: 104.18.100.28 [美国 CloudFlare 公司 CDN 节点]
Name:   blog.skk.moe
Address: 2606:4700::6812:641c
Name:   blog.skk.moe
Address: 2606:4700::6812:651c


$ dig cdn.jsdelivr.net @1.0.0.1 +short

cdn.jsdelivr.net.cdn.cloudflare.net. [Cloudflare]
104.16.88.20 [美国，CloudFlare]
104.16.89.20 [美国，CloudFlare]
104.16.85.20 [美国，CloudFlare]
104.16.87.20 [美国，CloudFlare]
104.16.86.20 [美国，CloudFlare]
```

> Nali CLI has built-in tools:
>
> - `nali-dig`
> - `nali-nslookup`
> - `nali-ping`
> - `nali-tracepath`
> - `nali-traceroute`
>
> Nali required related software installed. For example, in order to use `nali-dig` and `nali-nslookup` you need to have bind (dnsutils) installed.

Change Query Server, Persist Records To `~/.config/nali-cli/servername.txt`:

```bash
$ nali server
Current query server is [taobao]

? Select a query server:  (Use arrow keys)
❯ taobao
  ipinfo.io
  ipapi.is
  ip.sb
  ip.fm
  ip-api
  ipstack
  ipdata
  ipwho
  ipregistry

# View the current query server
$ nali server -c

# Change query server to default
$ nali server -d
```

## Interface

```bash
$ nali --help

Usage: nali <command> [options]

Options:
  -v, --version  版本信息
  -h, --help     output usage information

Commands:
  parse          解析 stdin 或参数中的 IP 信息 (默认)
  server         更改 IP 查询服务器
  help [cmd]     display help for [cmd]
```

## Related
- [Nali-cli](https://github.com/SukkaW/nali-cli) Forked From, Nali CLI, query with qqwry offline
- [Nali](https://github.com/SukkaW/Nali) Oringinal Nali CLI, written in C & Perl
- [Commander.js](https://github.com/tj/commander.js) Node.js command-line interfaces made easy
- [SukkaLab/cdn](https://lab.skk.moe/cdn) A CDN CNAME Data
