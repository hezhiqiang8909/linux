###ping命令常用方法

**1、探测网络**

    ping www.baidu.com

**2、指定ping 个数**

    ping -c 10 www.baidu.com

**3、指定ping 的间隔时间**

    ping -i 0.1  www.baidu.com

**4、指定网络接口**

    ping -I eth0 www.baidu.com
    
**5、指定ping 包大小**
    
    ping -s 60000 www.baidu.com

**6、快速ping**

    ping -f www.baidu.com
    
**7、ping攻击**

    ping -f -s 60000 XXX
 
**8、ping 广播地址**   

    ping -b broadcast

**9、禁|解ping**

    禁止：
    echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all
    
    允许： 
    echo 0 > /proc/sys/net/ipv4/icmp_echo_ignore_all 
    