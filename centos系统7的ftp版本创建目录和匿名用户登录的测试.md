
> centos6系统使用的ftp是2.0以上的版本，而centos7使用的是3.0.2版本
## 首先说一下ftp使用匿名用户创建目录
在centos7下只能在pub下创建文件夹，而且pub用户属主必须是ftp或者将pub目录授予777的权限，也就是说ftp用户在pub目录下必须具有写的权限。
![图一](http://peofbsx9u.bkt.clouddn.com/18-9-13/63487703.jpg)
在这之前需要确保配置文件这几个选项为YES，这几个选项在配置文件里都是默认开启的，还有selinux要处于Permissive状态
```
anon_mkdir_write_enable=YES
anon_upload_enable=YES
anonymous_enable=YES
```
## 还有centos7下拒绝访问ftp服务的权限
首先，ftp的访问跟ftp/pub的权限是没关系的，在正常情况下，授予ftp目录777的权限，是拒绝用户访问的
![图二](http://peofbsx9u.bkt.clouddn.com/18-9-13/64377833.jpg)

如果将ftp目录的属主和属组都修改成ftp用户，将ftp目录的权限修改回755，也是不能访问的

![图三](http://peofbsx9u.bkt.clouddn.com/18-9-13/10904134.jpg)

将配置文件的umask修改成000，并且重启服务，还是不能访问，但是如果我将ftp的目录的属主和属组都修改成ftp，而ftp的目录权限修改成557，那么ftp服务就可以正常访问了

![图四](http://peofbsx9u.bkt.clouddn.com/18-9-13/98588304.jpg)

当我把ftp的属主和属组又修改成root之后，ftp服务又不能访问了

![图五](http://peofbsx9u.bkt.clouddn.com/18-9-13/77312475.jpg)

我再把ftp的属主和属组都修改成ftp,,ftp服务还是可以正常的访问

![图六](http://peofbsx9u.bkt.clouddn.com/18-9-13/61939290.jpg)

如果我再将ftp的目录权限修改成335，而属主和属组都是root，那么ftp也是可以访问的

![图七](http://peofbsx9u.bkt.clouddn.com/18-9-13/16375443.jpg)

保持权限不变，而属主和属组改为ftp，那么ftp服务还是不能访问

![图八](http://peofbsx9u.bkt.clouddn.com/18-9-13/87405128.jpg)

我再修改ftp目录的权限为445，将ftp目录的属主和属组都改为root，ftp服务还是可以访问的

![图九](http://peofbsx9u.bkt.clouddn.com/18-9-13/93471053.jpg)

ftp目录的权限不变，修改ftp目录的属主和属组为ftp,ftp是可以进行访问的

![图十](http://peofbsx9u.bkt.clouddn.com/18-9-13/72316895.jpg)

这里属主和属组都不改变，分别修改ftp目录的权限为665和225，结果ftp服务都不能访问

![图十一](http://peofbsx9u.bkt.clouddn.com/18-9-13/78582627.jpg)
![图十二](http://peofbsx9u.bkt.clouddn.com/18-9-13/9760739.jpg)

也就是说不能给ftp用户写的权限，否则不能访问ftp。那如果我修改一下默认目录试试呢，我将ftp默认访问路径修改成/var/test，再次进行测试发现test目录的权限也是不能有写权限，否则不能访问

![图十三](http://peofbsx9u.bkt.clouddn.com/18-9-13/29240569.jpg)
![图十四](http://peofbsx9u.bkt.clouddn.com/18-9-13/10166450.jpg)

> 因此，我们可以得出结论：访问ftp服务的匿名用户不能有写权限，这也就是为什么ftp目录下匿名用户不能创建子目录的原因

