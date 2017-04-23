# Ubuntu 17.04 + Snort 2.9.9.0 + Barnyard2 + PulledPork + BASE

### 官方提供了详细的环境搭建指导，建议参照Snort 2.9.9.x on Ubuntu 14 and 16.PDF自己动手安装，对工具有个粗略的了解<br>

### VM 虚拟机 镜像下载<br>
<br>
#### 链接：http://pan.baidu.com/s/1o78Jsp0 密码：ursq
<br>
虚拟机root密码 “a123456”<br>
snort 2.9.9.0 + Barnyard2 + mysql(root 5175982903 ; snort 123456) + BASE + PulledPork(DOWNLOAD MD5 校验失败 待修复 可手动下载)<br>

## 简单命令
#rules file: /etc/snort/rules/local.rules<br>
```
alert icmp any any -> $HOME_NET any (msg:"ICMP test detected"; GID:1; sid:10000001; rev:001; classtype:icmp-event;)
```
#To make sure that barnyard2 knows that the rule we created with unique identifier 10000001 has the #message ”ICMP Test Detected”, as well as some other information (please see this blog post for more information). We add the following line #to the #/etc/snort/sid-msg.map file:<br>
```
1 || 10000001 || 001 || icmp-event || 0 || ICMP Test detected || url,tools.ietf.org/html/rfc792
```
运行输出到控制台
```
sudo /usr/local/bin/snort -A console -q -u snort -g snort -c /etc/snort/snort.conf -i eth0
```
Run Snort in alert mode<br>
```
sudo /usr/local/bin/snort -q -u snort -g snort -c /etc/snort/snort.conf -i eth0
```
Run Barnyard2<br>
```
sudo barnyard2 -c /etc/snort/barnyard2.conf -d /var/log/snort -f snort.u2 -w /var/log/snort/barnyard2.waldo \ -g snort -u snort
```
Check the MySQL database<br>
```
mysql -u snort -p -D snort -e "select count(*) from event"
```
## 存在的问题
