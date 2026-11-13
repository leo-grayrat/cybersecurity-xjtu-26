## HOST配置

![image-20261112230226500](./DNS实验.assets/image-20261112230226500.png)

## dig 工具

### dig 解析

> 一开始IPv4网络提前配置到了后续要求的网络（IPv4 地址192.168.187.130 子网掩码255.255.255.0），结果失败。
>
> ```
> dnsserver@server:~$ dig www.baidu.com
> 
> ; <<>> DiG 9.10.3-P4-Ubuntu <<>> www.baidu.com
> ;; global options: +cmd
> ;; Got answer:
> ;; ->>HEADER<<- opcode: QUERY, status: SERVFAIL, id: 39821
> ;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 0, ADDITIONAL: 1
> 
> ;; OPT PSEUDOSECTION:
> ; EDNS: version: 0, flags:; udp: 4096
> ;; QUESTION SECTION:
> ;www.baidu.com.			IN	A
> 
> ;; Query time: 880 msec
> ;; SERVER: 127.0.1.1#53(127.0.1.1)
> ;; WHEN: Thu Nov 12 07:04:55 PST 2026
> ;; MSG SIZE  rcvd: 42
> ```
>

设置为IPv4直连后反而是正常的。

![image-20261112231416470](./DNS实验.assets/image-20261112231416470.png)

### root 根服务器

`+trace` 可查看逐级解析，追踪从 DNS 根开始的逐级查询。
$$
\text{根域名服务器}\\\implies

\text{.com 顶级域服务器}\\\implies

\text{bilibili.com 权威服务器}\\\implies

\text{得到最终解析结果}
$$

```
dnsserver@server:~$ dig +trace www.bilibili.com

; <<>> DiG 9.10.3-P4-Ubuntu <<>> +trace www.bilibili.com
;; global options: +cmd
.			5	IN	NS	e.root-servers.net.
.			5	IN	NS	a.root-servers.net.
.			5	IN	NS	b.root-servers.net.
.			5	IN	NS	f.root-servers.net.
.			5	IN	NS	i.root-servers.net.
.			5	IN	NS	h.root-servers.net.
.			5	IN	NS	m.root-servers.net.
.			5	IN	NS	c.root-servers.net.
.			5	IN	NS	j.root-servers.net.
.			5	IN	NS	g.root-servers.net.
.			5	IN	NS	d.root-servers.net.
.			5	IN	NS	l.root-servers.net.
.			5	IN	NS	k.root-servers.net.
.			5	IN	RRSIG	NS 8 0 518400 20261124200000 20261111190000 8763 . cjQwebQ8nHIc6NzUhyR5ln5dtivAcCu4z257Jb7Z/JS8zwIsjMcTnc4c Is+E4ce7akuVYaEgxS2Zy8e8RnGgM0f+TmBNcClSRj7xiAtjOoWyANZW h+nCEvVS1h3ph1UeuCGB0B9kfkOp3v3RHCGaK3nb3PN+rV2916/VvSZe iVSEDNMkK2Y13fPIB1z9wRWf4kf8xHrRXle3QqrUYgl1Jj7bCGMeeCRT 6am+hURZKj6g7SI1bxyXONEKGFLgyzigj2VRO5jNKVgb4rOvF0aKqjzd 8unIdPw45ZKNH6IMwXmusTPLl1EulOf6wtJIKvd69gSMEdWKg8Ns0au+ YOcoQg==
;; Received 1097 bytes from 127.0.1.1#53(127.0.1.1) in 18 ms

com.			172800	IN	NS	i.gtld-servers.net.
com.			172800	IN	NS	a.gtld-servers.net.
com.			172800	IN	NS	g.gtld-servers.net.
com.			172800	IN	NS	j.gtld-servers.net.
com.			172800	IN	NS	h.gtld-servers.net.
com.			172800	IN	NS	l.gtld-servers.net.
com.			172800	IN	NS	f.gtld-servers.net.
com.			172800	IN	NS	b.gtld-servers.net.
com.			172800	IN	NS	c.gtld-servers.net.
com.			172800	IN	NS	e.gtld-servers.net.
com.			172800	IN	NS	m.gtld-servers.net.
com.			172800	IN	NS	d.gtld-servers.net.
com.			172800	IN	NS	k.gtld-servers.net.
com.			86400	IN	DS	19718 13 2 8ACBB0CD28F41250A80A491389424D341522D946B0DA0C0291F2D3D7 71D7805A
com.			86400	IN	RRSIG	DS 8 1 86400 20261125050000 20261112040000 8763 . Y21HuPWT7W0hRN+xU4WZgDWjNBfV/kCdfnMZWZ/JXrM5DgdmkR+x9w6S wOGWp2LUOtoWeNO5m0+2IUdAow1I+EyWXyJczWmLw80jDm+9GqWeHXmQ UK3S/hEZUFI+9AEnm8OYuW0PtxIE2TJ1i06QWAYaRSPliwzUe/oeBH9N e3sgZCs/k7gZnnw9p48Uq8WYZEvwkKsLUexBHSLJoONrlAVpVTYCioTG 4YDv2VL9/4PZRQAJW7RJOadnO1CLeYPCQjrwfWQM37GF6uzJkcMUSb2z 854XhlfgVC3erBhoffCKVP+PwPRYduzBUhklMcucWF1vWvz+v5b+u+W3 UPg5Vw==
;; Received 1176 bytes from 202.12.27.33#53(m.root-servers.net) in 8 ms

bilibili.com.		172800	IN	NS	ns3.dnsv5.com.
bilibili.com.		172800	IN	NS	ns4.dnsv5.com.
CK0POJMG874LJREF7EFN8430QVIT8BSM.com. 900 IN NSEC3 1 1 0 - CK0Q35HRS4H76G6CHNB9414CJN6S5UPL NS SOA RRSIG DNSKEY NSEC3PARAM
CK0POJMG874LJREF7EFN8430QVIT8BSM.com. 900 IN RRSIG NSEC3 13 2 900 20261118002605 20261110231605 65344 com. ZlJH4zBTd8732a1sdMrM1t1uuJ5HV9bhZ2nkKH/fObF0lZ4F2Xq3/iVy jLN99usMe6ipC1s+h7vgkJN6UzsdYA==
34N8IUNMFDGUL13GCNB325CKVM924592.com. 900 IN NSEC3 1 1 0 - 34N8VOVER4CALDU6ILDJ57NE037S5FB1 NS DS RRSIG
34N8IUNMFDGUL13GCNB325CKVM924592.com. 900 IN RRSIG NSEC3 13 2 900 20261119031402 20261112020402 65344 com. ERSfmtMMGCl+c+k7FmsQ5qxcWE24RCXaebATyIQejXnDwUIK2pEklYT4 CYThe291Hd83m2K8ELutHfDmDwGiVQ==
;; Received 772 bytes from 192.31.80.30#53(d.gtld-servers.net) in 189 ms

www.bilibili.com.	300	IN	CNAME	a.w.bilicdn1.com.
;; Received 75 bytes from 101.227.168.52#53(ns3.dnsv5.com) in 68 ms
```

### .com 服务器

`NS` 是 域名服务器记录。

```
dnsserver@server:~$ dig NS com

; <<>> DiG 9.10.3-P4-Ubuntu <<>> NS com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 32211
;; flags: qr rd ra; QUERY: 1, ANSWER: 13, AUTHORITY: 0, ADDITIONAL: 12

;; QUESTION SECTION:
;com.				IN	NS

;; ANSWER SECTION:
com.			5	IN	NS	i.gtld-servers.net.
com.			5	IN	NS	m.gtld-servers.net.
com.			5	IN	NS	h.gtld-servers.net.
com.			5	IN	NS	j.gtld-servers.net.
com.			5	IN	NS	g.gtld-servers.net.
com.			5	IN	NS	b.gtld-servers.net.
com.			5	IN	NS	l.gtld-servers.net.
com.			5	IN	NS	e.gtld-servers.net.
com.			5	IN	NS	c.gtld-servers.net.
com.			5	IN	NS	k.gtld-servers.net.
com.			5	IN	NS	d.gtld-servers.net.
com.			5	IN	NS	f.gtld-servers.net.
com.			5	IN	NS	a.gtld-servers.net.

;; ADDITIONAL SECTION:
a.gtld-servers.net.	5	IN	A	192.5.6.30
a.gtld-servers.net.	5	IN	AAAA	2001:503:a83e::2:30
b.gtld-servers.net.	5	IN	A	192.33.14.30
b.gtld-servers.net.	5	IN	AAAA	2001:503:231d::2:30
c.gtld-servers.net.	5	IN	A	192.26.92.30
c.gtld-servers.net.	5	IN	AAAA	2001:503:83eb::30
d.gtld-servers.net.	5	IN	A	192.31.80.30
d.gtld-servers.net.	5	IN	AAAA	2001:500:856e::30
e.gtld-servers.net.	5	IN	A	192.12.94.30
e.gtld-servers.net.	5	IN	AAAA	2001:502:1ca1::30
f.gtld-servers.net.	5	IN	A	192.35.51.30
f.gtld-servers.net.	5	IN	AAAA	2001:503:d414::30

;; Query time: 58 msec
;; SERVER: 127.0.1.1#53(127.0.1.1)
;; WHEN: Thu Nov 12 07:19:10 PST 2026
;; MSG SIZE  rcvd: 509
```

### bilibili.com 服务器

查询对应域名的 NS 记录。

```
dnsserver@server:~$ dig NS bilibili.com

; <<>> DiG 9.10.3-P4-Ubuntu <<>> NS bilibili.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 10573
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 16

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; MBZ: 0005 , udp: 4096
;; QUESTION SECTION:
;bilibili.com.			IN	NS

;; ANSWER SECTION:
bilibili.com.		5	IN	NS	ns4.dnsv5.com.
bilibili.com.		5	IN	NS	ns3.dnsv5.com.

;; ADDITIONAL SECTION:
ns3.dnsv5.com.		5	IN	A	220.196.136.52
ns3.dnsv5.com.		5	IN	A	1.12.0.17
ns3.dnsv5.com.		5	IN	A	1.12.0.18
ns3.dnsv5.com.		5	IN	A	1.14.119.52
ns3.dnsv5.com.		5	IN	A	36.155.149.52
ns3.dnsv5.com.		5	IN	A	43.140.237.52
ns3.dnsv5.com.		5	IN	A	101.227.168.52
ns3.dnsv5.com.		5	IN	AAAA	2402:4e00:1730::f
ns4.dnsv5.com.		5	IN	A	43.140.78.52
ns4.dnsv5.com.		5	IN	A	49.7.107.152
ns4.dnsv5.com.		5	IN	A	117.135.128.152
ns4.dnsv5.com.		5	IN	A	1.12.0.16
ns4.dnsv5.com.		5	IN	A	1.12.0.19
ns4.dnsv5.com.		5	IN	A	1.14.119.54
ns4.dnsv5.com.		5	IN	AAAA	2402:4e00:1231::8

;; Query time: 10 msec
;; SERVER: 127.0.1.1#53(127.0.1.1)
;; WHEN: Thu Nov 12 07:19:10 PST 2026
;; MSG SIZE  rcvd: 347
```

### 服务器地址

`A` 是 IPv4 地址记录。查询B站服务器地址。

```
dnsserver@server:~$ dig www.bilibili.com

; <<>> DiG 9.10.3-P4-Ubuntu <<>> www.bilibili.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 42970
;; flags: qr rd ra; QUERY: 1, ANSWER: 5, AUTHORITY: 2, ADDITIONAL: 16

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; MBZ: 0005 , udp: 4096
;; QUESTION SECTION:
;www.bilibili.com.		IN	A

;; ANSWER SECTION:
www.bilibili.com.	5	IN	CNAME	a.w.bilicdn1.com.
a.w.bilicdn1.com.	5	IN	A	121.194.11.75
a.w.bilicdn1.com.	5	IN	A	121.194.11.73
a.w.bilicdn1.com.	5	IN	A	121.194.11.74
a.w.bilicdn1.com.	5	IN	A	121.194.11.72

;; AUTHORITY SECTION:
bilicdn1.com.		5	IN	NS	ns3.dnsv5.com.
bilicdn1.com.		5	IN	NS	ns4.dnsv5.com.

;; ADDITIONAL SECTION:
ns3.dnsv5.com.		5	IN	A	1.12.0.18
ns3.dnsv5.com.		5	IN	A	1.14.119.52
ns3.dnsv5.com.		5	IN	A	36.155.149.52
ns3.dnsv5.com.		5	IN	A	43.140.237.52
ns3.dnsv5.com.		5	IN	A	101.227.168.52
ns3.dnsv5.com.		5	IN	A	220.196.136.52
ns3.dnsv5.com.		5	IN	A	1.12.0.17
ns3.dnsv5.com.		5	IN	AAAA	2402:4e00:1730::f
ns4.dnsv5.com.		5	IN	A	1.14.119.54
ns4.dnsv5.com.		5	IN	A	43.140.78.52
ns4.dnsv5.com.		5	IN	A	49.7.107.152
ns4.dnsv5.com.		5	IN	A	117.135.128.152
ns4.dnsv5.com.		5	IN	A	1.12.0.16
ns4.dnsv5.com.		5	IN	A	1.12.0.19
ns4.dnsv5.com.		5	IN	AAAA	2402:4e00:1231::8

;; Query time: 38 msec
;; SERVER: 127.0.1.1#53(127.0.1.1)
;; WHEN: Thu Nov 12 07:19:12 PST 2026
;; MSG SIZE  rcvd: 442

```

## DNS投毒实验

### 启动BIND9

> 值得注意的是，有次开机时出现了下文报错
>
> ```
> dnsserver@server:~$ sudo apt-get install bind9
> [sudo] password for dnsserver: 
> E: Could not get lock /var/lib/dpkg/lock-frontend - open (11: Resource temporarily unavailable)
> E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), is another process using it?
> ```
>
> 而在此之前仅仅开机并运行了 `ls -l /etc/bind/` 。
>
> 检查后发现确实有占用锁，但是当时忘记保存截图从而无从得知为何光板的 Ubuntu 刚开机会有这种情况。

![image-20261114005154397](./DNS实验.assets/image-20261114005154397.png)

### 配置文件

#### 设置 `named.conf.options`

> 值得注意的是，文档中给出了 vim 编辑的方法，然而对于 linux 小白来说 vim 并不易使用。
>
> 而如果直接点开（也就是用 **gedit** 编辑器）修改会发现无法保存，因为大多数情况本实验内碰的都是系统文件。
>
> 因此需要管理员权限 `sudo` 的 `gedit` 打开。
>
> ```
> sudo gedit named.conf.options
> ```
>
> ~~有点搞笑，刚写完这个就发现后面文档里面也提到了这一点~~

![image-20261114014353366](./DNS实验.assets/image-20261114014353366.png)

```
//要求注释
	//dnssec-validation auto;

	dump-file "/var/cache/bind/dump.db"; // 用于指定输出位置
	query-source port 33333;			// 指定发送请求包时使用的端口
	dnssec-enable no;					// 禁用安全机制
```

- `dump-file` 指定 DNS 缓存**转储文件**的位置。
  - BIND9 平时把缓存保存在内存等运行状态中，需要用 `rndc dumpdb` 主动生成一个可供检查的转储文件。
- `query-source port 33333` 指定这台 DNS 服务器向上游发出查询时使用的 **UDP 来源端口**。
  - 为了研究旧式 DNS 缓存投毒把端口固定为 `33333`，这样实验条件更容易分析。
  - 这是**降低安全性**的实验配置，一般的 DNS 服务器正常部署显然是会随机化来源端口的。
- `dnssec-enable no` 关闭 BIND9 的 DNSSEC 支持，并注释自动验证配置。

> GPT 表示：
>
> DNSSEC 可以通过密码学验证某些 DNS 数据的真实性，阻止攻击者仅凭伪造记录欺骗验证解析器。
>
> 不过，这里有两个细节：
>
> - `dnssec-enable` 是旧版配置选项，在现代 BIND9 中可能已经被删除。
> - 注释 `dnssec-validation auto` 与明确配置 `dnssec-validation no` 并不总是等价的。
>
> 因此，不能看到配置保存成功就认为 DNSSEC 一定被关闭，应以实际版本的配置检查和运行行为为准。

![image-20261114021732570](./DNS实验.assets/image-20261114021732570.png)

下面根据 [[计算机网络安全实验] DNS攻击实验](https://blog.csdn.net/LostUnravel/article/details/127819578) 补全了正向区域和反向区域。

#### 正向区域

| 记录             | 类型 | 目标值                             |
| ---------------- | ---- | ---------------------------------- |
| example.com      | SOA  | ns.example.com / admin.example.com |
| example.com      | NS   | ns.example.com                     |
| example.com      | MX   | 10 mail.example.com                |
| www.example.com  | A    | 192.168.0.101                      |
| mail.example.com | A    | 192.168.0.102                      |
| ns.example.com   | A    | 192.168.0.10                       |
| *.example.com    | A    | 192.168.0.100                      |

**SOA** “**区域管理信息**”……是什么？其实就是说明 **DNS 区域**的一系列**基本信息**

```dns
example.com. IN SOA ns.example.com. admin.example.com. (
    2008111001
    8H
    2H
    4W
    1D
)
```

- `ns.example.com.`：这个区域的**主（首选）名称服务器**（该 DNS 区域由谁管，只有**一台**）
- `admin.example.com.`：**管理员联系方式**邮箱（DNS 里用第一个点代替 `@`）
- `2008111001`：区域序列号
- `8H`：刷新间隔，从服务器多久检查一次更新
- `2H`：重试间隔，同步失败后多久再试
- `4W`：过期时间，从服务器联系不上主服务器多久后认为数据过期
- `1D`：负面缓存 TTL 相关值，表示查询失败的结果能缓存多久

---

**NS** 表示 此区域的**权威名称服务器**（可以有**很多**，只表示“他们是权威的”）

> 举例：
>
> ```dns
> example.com. IN SOA ns1.example.com. admin.example.com. (
>     2024010101 8H 2H 4W 1D
> )
> example.com. IN NS ns1.example.com.
> example.com. IN NS ns2.example.com.
> ns1.example.com. IN A 192.168.0.10
> ns2.example.com. IN A 192.168.0.11
> ```
>
> 这里：
>
> - **SOA MNAME = `ns1.example.com`**：表示 `ns1` 是主服务器/首选源，常用于区域传输、通知、动态更新等协议场景。
> - **NS = `ns1` 和 `ns2`**：表示 `example.com` 有**两个权威名称服务器**，对外都说“我负责这个区域”。

---

MX 表示 此域名的**邮件收件服务器**。这是非必须的，同时~~也不是要给DNS发邮件的意思~~

```dns
example.com. IN MX 10 mail1.example.com.
example.com. IN MX 20 mail2.example.com.
```

此处数字表示优先级，会先尝试数字小的邮件地址。

---

A 则很简单，就表示**域名对应的 IPv4 地址**。

```dns
*.example.com. IN A 192.168.0.100
```

\*是**通配符**记录，使没有单独定义的子域名也能得到预设地址。

#### 反向区域

| IP 尾段 | PTR 记录         |
| ------- | ---------------- |
| 101     | www.example.com  |
| 102     | mail.example.com |
| 10      | ns.example.com   |

- **正向区域**：域名 → IP 地址， `www.example.com → 192.168.0.101`
- **反向区域**：IP 地址 → 域名， `192.168.0.101 → www.example.com`

具体地，DNS 的反向查询树是按 IP 地址字节**倒序**组织的。

> GPT：
>
> 需要注意：创建反向区域数据文件，并不会自动让 BIND9 对外提供反向解析。必须在配置文件中声明该区域。
>
> 而且原文后面的实验把反向区域声明注释掉了，所以在该状态下，即使文件存在，反向解析也可能无法使用。
