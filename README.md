# zaldap

# 20. zaldap 监控部署

        通过zabbix agent要获取openldap的监控参数，需要开启了openldap的监控选项。
    有2种方法一种是通过 cn=config开启，另外一种是需要slapd.conf(需要重启openldap)

## 20.1. Monitor configuration via cn=config

    #~ vi monitor_enable.ldif
    dn: cn=module,cn=config
    cn: module
    objectClass: olcModuleList
    olcModuleLoad: back_monitor
    olcModulePath: /usr/lib/ldap

    dn: olcDatabase={2}monitor,cn=config
    objectClass: olcDatabaseConfig
    olcDatabase: {2}monitor
    olcAccess: {0}to * by dn.exact=cn=monitor,ou=auth,dc=example,dc=com manage by * none

    #~ ldapadd -Q -Y EXTERNAL -H ldapi:/// -f monitor_enable.ldif
    #~

## 20.2. Monitor configuration via slapd.conf

    #~ vi /etc/ldap/slapd.conf
    ...
    database monitor
    
    access to *
           by dn.exact=cn=monitor,ou=auth,dc=example,dc=com manage
           by * none
    #~

# Zabbix deploy 
    部署zaldap监控插件可以使用，插件目录自带部署脚本进行部署。
     
     deploy_zabbix.sh 有三个参数：
     
       第一个参数：openldap 服务器地址
       
       第二个参数：openldap 监控用户
       
       第一个参数：openldap 监控用户的密码

      
      
        #~ git clone https://github.com/sergiotocalini/zaldap.git
    
        #~ ./zaldap/deploy_zabbix.sh 'localhost' 'cn=monitor,ou=auth,dc=example,dc=com' 'xxxxxxxx'
    
        #~
    
    
    
    
