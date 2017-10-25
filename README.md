在单台  服务器部署2个docker 容器


首先 创建个MySQL容器 让他链接 

docker  run -itd    --name mysql  -e MYSQL_ROOT_PASSWORD=264333 mysql


    docker run  -itd  --name ns1.nb03.com -p  8080:80 --link mysql:db  -e PDNS_ALLOW_AXFR_IPS=172.17.0.4  -e  PDNS_MASTER=yes -e POWERADMIN_HOSTMASTER=ns1.nb03.com -e POWERADMIN_NS1=ns1.nb03.com -e POWERADMIN_NS2=ns2.nb03.com  secns/pdns:4.0
    docker run  -itd  --name ns2.nb03.com  --link mysql2:db  -p 8089:80   -e PDNS_MASTER=no -e  PDNS_SLAVE=yes -e POWERADMIN_HOSTMASTER=ns2.nb03.com -e POWERADMIN_NS1=ns1.nb03.com -e POWERADMIN_NS2=ns2.nb03.com  secns/pdns:4.0


	找一台做  master 主控。 
	其他的都是做slave 这样方便管理
	
	然后在每台slave  插入 supermaster  数据。 或者直接在   每台slave 的web 面板添加 add  supermaster
	
	
	
主master   PDNS_ALLOW_AXFR_IPS=172.17.0.4    这里就是允许更新的slave IP。 多个IP 用  PDNS_ALLOW_AXFR_IPS="172.17.0.4 172.17.0.5"      用双引号 和空格隔开







PowerDNS Authoritative server and Poweradmin
===========
[![](https://badge.imagelayers.io/secns/pdns:latest.svg)](https://imagelayers.io/?images=secns/pdns:latest 'Get your own badge on imagelayers.io')

# Quickstart

```wget https://raw.githubusercontent.com/obi12341/docker-pdns/master/docker-compose.yml && docker-compose up -d```

# Running

Just use this command to start the container. PowerDNS will listen on port 53/tcp, 53/udp and 8080/tcp.

```docker run --name pdns-master --link mysql:db -d -p 53:53/udp -p 53:53 -p 8080:80 secns/pdns:4.0```

Login:
``` admin / admin ```

# Configuration
These options can be set:

- **PDNS_ALLOW_AXFR_IPS**: restrict zonetransfers to originate from these IP addresses. Enter your slave IPs here. (Default: "127.0.0.1", Possible Values: "IPs comma seperated")
- **PDNS_MASTER**: act as master (Default: "yes", Possible Values: "yes, no")
- **PDNS_SLAVE**: act as slave (Default: "no", Possible Values: "yes, no")
- **PDNS_CACHE_TTL**: Seconds to store packets in the PacketCache (Default: "20", Possible Values: "<integer>")
- **PDNS_DISTRIBUTOR_THREADS**: Default number of Distributor (backend) threads to start (Default: "3", Possible Values: "<integer>")
- **PDNS_RECURSIVE_CACHE_TTL**: Seconds to store packets in the PacketCache (Default: "10", Possible Values: "<integer>")
- **PDNS_RECURSOR**: If recursion is desired, IP address of a recursing nameserver (Default: "no", Possible Values: "yes, no")
- **PDNS_ALLOW_RECURSION**: List of subnets that are allowed to recurse (Default: "127.0.0.1", Possible Values: "<ipaddr>")
- **POWERADMIN_HOSTMASTER**: default hostmaster (Default: "", Possible Values: "<email>")
- **POWERADMIN_NS1**: default Nameserver 1 (Default: "", Possible Values: "<domain>")
- **POWERADMIN_NS2**: default Nameserver 2 (Default: "", Possible Values: "<domain>")
