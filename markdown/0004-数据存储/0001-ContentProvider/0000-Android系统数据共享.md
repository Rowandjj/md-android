Content Provider作为Android应用程序中的四大组件之一，主要是为了实现在各应用程序之间数据共享，增强应用程序的复用，例如，在开发过程中，需要获取手机中的通讯录信息，这时完全不需要自己重新开发读取数据的整个过程，而是直接访问系统自带的Content Provider对象来直接获取数据（此例子只是说明有现成的能满足需要的ContentProvider即可随时“拿来”，只要有相应权限， 不用管它是来自哪个应用程序里）。
在Content Provider使用过程中，还需要借用ContentResolver对象作为代理，间接操作ContentProvider来获取数据。ContentProvider可以看作是对应着一个或多个数据表（类似查询后返回的数据集）。另外重要的概念还有Uri，通过它来找到相应的ContentProvider、并能定位到对应的数据表或字段。
相关的知识经过整理，特列出下面这张思维导图，以方便自己经常温故，也希望能给大家带来一丝思维上的提示，也算是抛砖引玉了，呵呵

![img](http://emanual.github.io/md-android/img/data_provider/01_datastore.jpg)