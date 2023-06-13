安装配置  
安装  

```
yum install -y subversion            # 安装
svnserve --version                   # 查看版本
```


公司有AD域控服务器，SVN可以设置直接用域控账户访问，就不用在SVN上去给用户创建账号，免去了不少麻烦，这时只需要对用户控制权限即可。

```
yum install -y mod_ldap      # 安装ldap模块
cd /etc/httpd/conf.d
vim svn.conf
-------------------------------------------------------------------
<Location /demo1>
DAV svn
SVNPath /data/svn/demo1

AuthType Basic
AuthName "Authorization Realm"
AuthzSVNAccessFile /data/svn/conf.d/authz
AuthBasicProvider ldap
AuthLDAPURL "ldap://172.16.1.2/OU="     # 根据实际的LADP服务结构填写
AuthLDAPBindDN "username"      # LDAP上的管理员用户
AuthLDAPBindPassword "password"     # 管理员的密码
Require valid-user
</Location>
-------------------------------------------------------------------

# 重启httpd
systemctl restart httpd

```

通过http即可访问
 

http://svn-dev.netease.com/svn/  
Authorization: Basic eWFuZ2dlMDE6ekZiV1NwSjU2clpROWM4dg==
