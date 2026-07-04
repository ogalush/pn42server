# Linux Router
## 概要
NEC IX2215 Routerを経由するとホームページを開くタイミングの微妙の待ちが気になるため入れ替えを考えた.  
今回は実験がてら、2026.6に退役させた ECS Livaを利用してLinuxRouterを試す.  
  
詳細: [Documents/LinuxRouter202607.md](https://github.com/ogalush/Documents/blob/master/LinuxRouter202607.md)  

## 構成
### 接続構成
```
[マンションインターネット]
↑
内蔵NIC 1000Base-T
[ECS Liva]
Baffalo LUA-U3-A2G/C (USB3.x) 2.5GBase-T
↑
[Planex FX2G-08EM2]
↑
SOLO10-TB3 2.5GBase-T
[MacBook Pro]
```
LAN: 192.168.3.0/24  
WAN: 100.64.0.0/22 + IPv6 (ndProxy + radvd)  

## セットアップ方法
ansible-playbookで設定する.
```
% ansible-playbook -i dev.ini install_linuxrouter.yml -bK --ask-vault-pass --list-hosts
% ansible-playbook -i dev.ini install_linuxrouter.yml -bK --ask-vault-pass --check --diff
% ansible-playbook -i dev.ini install_linuxrouter.yml -bK --ask-vault-pass
```

## 動作確認
* LAN → Internet
* IPv4 NAT
* IPv6 routing  
[あなたの IPv6 接続性をテストしましょう。](https://test-ipv6.com/index.html.ja_JP) でIPv6アドレスを拾えるか.
* DNS (unbound)
* WireGuard
* WAN IPv6 ping  
外からpingが返ってくること.
```
% ping6 -c 3 240b:10::...
→ 
```
IPv6ではICMPを頻繁に利用するため開けている.
* WAN TCP scan (all filtered)  
外からtcpのポートスキャンをして意図しないポートを開けてないこと.
```
% nmap -Pn -6 240b:10::...
Starting Nmap 7.99 ( https://nmap.org ) at 2026-07-05 04:31 +0900
Nmap scan report for 240b:10::...
Host is up.
All 1000 scanned ports on 240b:10::... are in ignored states.
Not shown: 1000 filtered tcp ports (no-response)

Nmap done: 1 IP address (1 host up) scanned in 403.66 seconds
```
  
以上