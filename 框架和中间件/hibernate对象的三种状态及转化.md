## hibernate对象状态
- 瞬时（Transient）：new出来的对象，在数据库没有与之对应的数据。和普通Pojo对象无区别。
- 持久（Persistent）：在数据库有与之对应的数据，同时对象在hibernate session缓存中管理。对Persistent对象修改属性，会自动刷入数据库中。
- 脱管/游离（Detached）：在数据库有与之对应的数据，但对象没有在hibernate session缓存中。对Detached对象修改属性，不会自动同步到数据库。

## hibernate对象转换条件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210720101953971.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25qemN4,size_16,color_FFFFFF,t_70)
## 自动刷新数据库问题
在实际开发过程中，会碰到从数据库查询对象，修改对象属性，在未调用update()时，会自动刷新到数据库。导致和实际预期不相符。
解决办法：<br>
1、查询持久化对象，将属性填充到一个new对象（Transient）中返回。<br>
2、清理hibernate session，由于session.close()和session.clear()影响范围过大，建议**使用session.evict(object)方法**，将持久化对象转换为detached对象。<br>

引用链接：<br>
[Hibernate 中文文档 3.2](https://wizardforcel.gitbooks.io/hibernate-doc/content/153.html) <br>
[Hibernate中对象的三种状态及相互转化](https://blog.csdn.net/FG2006/article/details/6436517) <br>

扩展链接：<br>
[hibernate持久化方法](https://blog.csdn.net/confirmaname/article/details/9290535)
