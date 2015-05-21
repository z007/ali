##centos LNMP环境配置
* 
更新软件
[update_source.tgz](http://help.aliyun.com/knowledge_detail.htm?knowledgeId=5974184 )

* Linux一键安装web环境全攻略 

 [视屏](http://help.aliyun.com/knowledge_detail.htm?spm=5176.788314854.3.4.CEdSHo&knowledgeId=5973945&categoryId=8314854) 
 
```
注意
  chmod 777 -R sh1.4.1/ 
  ll 看权限是否分配好 
  ./install
  卸载同样chmod 777 -R sh1.4.1/  
```
```
修改mysql允许远程访问
在安装好之后 在sh1.4.1/ 内 cat account.log 看FTP mysql密码  
mysql -uroot -p密码
use mysql;
show tables;
desc user;
select Host,user,password from user;
update user set Host='%' where Host='localhost';
   flush privileges; 自己生效  或者 service mysqld restart
mysql -uroot -p -hIP地址 （远程可以访问了）
已知root 密码修改root 密码
mysql -uroot -p mysql
mysql>update user set password=password("new_pass") where user="root";
mysql>flush privileges;
```

```
删除数据库
例如，删除名为 xhkdb的数据库：
mysql> drop database xhkdb;

```
[Mysql基础教程](http://c.biancheng.net/cpp/html/1446.html)



* 网站文件存放位置

```
sh1.4.1/ 一键安装的lnmp 默认跟根目录是/alidata/www/phpwin/ 下面 
任何文件放在www下而不是在/www/phpwind 打开浏览器是找到不到的
 可在/alidata/server/nginx/conf/vhosts/

打开浏览器输入正确地址出现403  
在/alidata/www/ 下
 ll 查看权限chown www:www -R  文件目录（www下的）


一键安装可以将 把phpmyadmin 放在了/alidata/www/phpwind
可是里面phpwind的版本较老 在phpwind 官网下载较新的
可以再 /alidata/ 目录下
rz -be phpwind.zip
unzip -x phpwind.zip
cd www
mv phpwind phpwindbak  备份
cd ..
ll 查看 用户与组
chown www:www -R upload 改变用户与组
mv upload phpwind
cp -rf phpwind www/  
这时 cp -rf phpwindbak/phpwind  phpwind/
就OK了
```
```
软连接 实现同一服务器下多站点用以一个phpmyadmin （提高安全性）
ln -s /home/wwwroot/phpmyadmin /home/wwwroot/database
http://rffan.info/244
一个服务器只装了一个网站刚开始
搞清phpmyadmin的位置
默认的的是/alidata/www/phpwind/phpmyadmin(即在站点的根目录下本教程的默认站点phpwind)http://IP/域名/phpmyadmin
就可打开 了
但是我后来又增加了几个站点
即同意IP对应不同域名导致了我用http://IP/phpmyadmin 出现了403错误服务器此时就不知道到底要返回那个站点了
解决方法 http://域名/phpmyadmin 就可以了
```




静态化
[教程1](http://help.aliyun.com/knowledge_list.htm?categoryId=8314854)
[教程2](http://help.aliyun.com/knowledge_detail.htm?spm=5176.788314854.3.5.FBAY9d&knowledgeId=5973947&categoryId=8314854) 

在增加nginx下增加网址 
[教程](http://bbs.aliyun.com/read/3189.html)
```
查看教程如果无此句加上
log_format www下的文件目录 '$remote_addr - $remote_user [$time_local] "$request" '
		'$status $body_bytes_sent "$http_referer" '
		'"$http_user_agent" "$http_x_forwarded_for"';

下面测试下我们配置的文件是否正确吧 
/alidata/server/nginx/sbin/nginx -t

/alidata/server/nginx/sbin/nginx -s reload

```


查找某个文件
```
比如phpmyadmin 知道文件名不知道在哪个位置 就在根目录下全盘查找
 find / -name phpmyadmin
模糊查询 含有关键字比如'srm'
find / -name '*srm*'
以srm开头
find / -name '*srm'
以srm结尾
find / -name 'srm*'
```

