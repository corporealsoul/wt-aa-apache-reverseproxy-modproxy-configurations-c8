### Setting Up Reverse Proxy,

**Apache / load_balancer :** http://192.168.40.129/

**Backend tomcat :** http://192.168.40.128:8080/

**Backend tomcat :** http://192.168.40.128:9090/

<br>

`[root@server ~]# httpd -M`

`[root@server ~]# grep "mod_proxy" /etc/httpd/conf.modules.d/00-proxy.conf`

`[root@server ~]# nano /etc/httpd/conf.d/reverse-proxy.conf`

<br>

### Reverse Proxying a Single Backend Server

    <VirtualHost *:80>
        ProxyPreserveHost On
    
        ProxyPass / http://192.168.40.128:8080/
        ProxyPassReverse / http://192.168.40.128:8080/
    </VirtualHost>

### Load Balancing Across Multiple Backend Servers

    <VirtualHost *:80>
    <Proxy balancer://mycluster>
        BalancerMember http://192.168.40.128:8080
        BalancerMember http://192.168.40.128:9090
    </Proxy>
    
        ProxyPreserveHost On
    
        ProxyPass / balancer://mycluster/
        ProxyPassReverse / balancer://mycluster/
    </VirtualHost>

<br>
