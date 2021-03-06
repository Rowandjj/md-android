在Android应用程序之间数据共享—-ContentResolver中，已经说明了Android是如何实现应用程序之间数据共享的，并详细解析了如何获取其他应用程序共享的数据。ContentProviders存储和检索数据，通过它可以让所有的应用程序访问到，这也是应用程序之间唯一共享数据的方法。那么如何将应用程序的数据暴露出去？
通过以前文章的学习，知道ContentResolver是通过ContentProvider来获取其他与应用程序共享的数据，那么ContentResolver与ContentProvider的接口应该差不多的。
其中ContentProvider负责
1、组织应用程序的数据； 
2、向其他应用程序提供数据； 
ContentResolver则负责
1、获取ContentProvider提供的数据； 
2、修改/添加/删除更新数据等； 
ContentProvider 是如何向外界提供数据的？
Android提供了ContentProvider，一个程序可以通过实现一个ContentProvider的抽象接口将自己的数据完全暴露出去，而且ContentProviders是以类似数据库中表的方式将数据暴露，也就是说ContentProvider就像一个“数据库”。那么外界获取其提供的数据，也就应该与从数据库中获取数据的操作基本一样，只不过是采用URI来表示外界需要访问的“数据库”。至于如何从URI中识别出外界需要的是哪个“数据库”，这就是Android底层需要做的事情了，不在此详细说。简要分析下ContentProvider向外界提供数据操作的接口：
```  
query(Uri, String[], String, String[], String)
insert(Uri, ContentValues)
update(Uri, ContentValues, String, String[])
delete(Uri, String, String[])
```
这些操作与数据库的操作基本上完全一样，在此不详细说，具体的解析可以参考Android Sqlite解析篇中的详细说明。需要特殊说明的地方是URI：
在URI的D部分可能包含一个_ID，这个应该出现在SQL语句中的，可以以种特殊的方式出现，这就要求我们在提供数据的时候，需要来额外关注这个特殊的信息。Android  SDK推荐的方法是：在提供数据表字段中包含一个ID，在创建表时INTEGER PRIMARY KEY AUTOINCREMENT标识此ID字段。
ContentProvider 是如何组织数据的？
组织数据主要包括：存储数据，读取数据，以数据库的方式暴露数据。数据的存储需要根据设计的需求，选择合适的存储结构，首选数据库，当然也可以选择本地其他文件，甚至可以是网络上的数据。数据的读取，以数据库的方式暴露数据这就要求，无论数据是如何存储的，数据最后必须以数据的方式访问。
可能还有2个问题，是需要关注的。
ContentProvider是什么时候创建的，是谁创建的？访问某个应用程序共享的数据，是否需要启动这个应用程序？这个问题在Android SDK中没有明确说明，但是从数据共享的角度出发，ContentProvider应该是Android在系统启动时就创建了，否则就谈不上数据共享了。这就要求在AndroidManifest.XML中使用<provider>元素明确定义。 
可能会有多个程序同时通过ContentResolver访问一个ContentProvider，会不会导致像数据库那样的“脏数据”？这个问题一方面需要数据库访问的同步，尤其是数据写入的同步，在AndroidManifest.XML中定义ContentProvider的时候，需要考虑是<provider>元素multiprocess属性的值；另外一方面Android在ContentResolver中提供了notifyChange()接口，在数据改变时会通知其他ContentObserver，这个地方应该使用了观察者模式，在ContentResolver中应该有一些类似register，unregister的接口。 
至此，已经对ContentProvider提供了比较全面的分析，至于如何创建ContentProvider，可通过2种方法：创建一个属于你自己的ContentProvider或者将你的数据添加到一个已经存在的ContentProvider中，当然前提是有相同数据类型并且有写入Content provider的权限。在Android SDK的sample中提供的Notepad具体实例中去看源代码！