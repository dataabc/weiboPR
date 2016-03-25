### 功能
- 判断微博用户的影响力

### 输入
- 用户id：例如新浪微博昵称为“Google黑板报”的id为“2617744132”。可以输入一个用户的user_id，也可以输入多个用户的user_id，具体的输入代码示例如下：
```python
user_ids = [1729370543,1644461042,1669879400,2617744132]
```
其中的数字可以替换成我们想要求的用户的id

### 输出
- 用户的pr值：用户的pr值最终存储在字典里，字典名字是我们自己取的，假如我们把pr值存储到字典user_pr里，user_pr[1729370543]即为id为1729370543的用户的pr值，pr值越大，代表用户的影响力越大。

### 求用户影响力的原理
　　我们知道，PageRank算法是用来判断网页权重的算法，通过指向页面的网页的权重，来计算页面的权重。我们也可以用类似的方法计算微博用户的影响力。比如，我们可以计算用户每个粉丝的权重，然后计算用户的权重，需要经过多次迭代。我们知道，部分用户的粉丝量非常大，如果用PageRank算法计算，效率会非常低，所以放弃了这种方法。<br>
　　那么什么因素与用户的影响力有关呢？第1个自然是粉丝数，粉丝数越大，用户的影响力很可能越大，因为微博中存在大量僵尸粉，所以单单看粉丝量是无法知道用户影响力的；第2个是微博平均被转发数，因为被转发了，说明微博自然是被关注了，而转发又意味着其他用户在帮着扩大影响力，所以是影响微博影响力的重要因素；第3个是微博平均被评论的数量；第4个是微博平均被点赞的数量。第2、3、4其实代表了粉丝与用户的互动情况，数量越多影响力越大。同时，在大多数情况下，一条微博的转发数要远远小于它被评论的数量，一条微博被评论的数量    又小于它被点赞的数量，所以它们对微博影响力的作用依次降低。所以，根据以上情况，我们可以为每个参数根据重要性赋一个权值，大致计算出用户的影响力，数值越大，用户的影响力也就越大。

### 运行环境
- 开发语言：python2.7
- 系统： windows 10（64位）
- 运行环境：ipython（Anaconda 64位）

### 使用说明
1、下载脚本
```bash
$ git clone https://github.com/dataabc/weibopr.git
```
运行上述命令，将本项目下载到当前目录，如果下载成功当前目录会出现一个名为"weibopr"的文件夹；<br>
2、用文本编辑器打开weiboPR文件夹下的"weiboPR.py"文件；<br>
3、将"weiboPR.py"文件中的“your cookie”替换成爬虫微博的cookie，后面会详细讲解如何获取cookie；<br>
4、将"weiboPR.py"文件中的user_id替换成想要爬取的微博的user_id，后面会详细讲解如何获取user_id；<br>
5、按需求调用脚本。本脚本是一个weibo类，用户可以按照自己的需求调用weibo类。
例如用户可以直接在"weiboPR.py"文件中调用weibo类，具体调用代码示例如下：
```python
	user_ids = [1729370543,1644461042,1669879400,2617744132] #替换成我们目标用户的id
	user_pr = {} #存储用户的pr值，用来判断用户的影响力，pr值越大代表影响力越高
	filter = 1 #值为0表示爬取全部的微博信息（原创微博+转发微博），值为1表示只爬取原创微博
	for i in user_ids:
		 wb = weibo(i,filter) #调用weibo类，创建微博实例wb
		 wb.start() #爬取微博信息
		 user_pr[i] = wb.pr #获得用户id为i的pr值
		 wb.writeTxt() #wb.writeTxt()只是把信息写到文件里，大家可以根据自己的需要重新编写writeTxt()函数
	print user_pr
```
user_id可以改成任意合法的用户id（爬虫的微博id除外）；filter默认值为0，表示爬取所有微博信息（转发微博+原创微博），为1表示只爬取用户的所有原创微博；wb是weibo类的一个实例，也可以是其它名字，只要符合python的命名规范即可；通过执行wb.start() 完成了微博的爬取工作。在上述代码之后，user_pr就存储了对应id用户的pr值；
6、运行脚本。我的运行环境是ipython,通过
```bash
$ run filepath/weiboPR.py
```
即可运行脚本，大家可以根据自己的运行环境选择运行方式。
###如何获取cookie
1、用Chrome打开<https://passport.weibo.cn/signin/login>；<br>
2、按F12键打开Chrome开发者工具；<br>
3、点开“Network”，将“Preserve log”选中，输入微博的用户名、密码，登录，如图所示：
![](http://7xknyo.com1.z0.glb.clouddn.com/github/weibospider/cookie1.png)
4、点击Chrome开发者工具“Name"列表中的"m.weibo.cn",点击"Headers"，其中"Request Headers"下，"Cookie"后的值即为我们要找的cookie值，复制即可，如图所示：
![](http://7xknyo.com1.z0.glb.clouddn.com/github/weibospider/cookie2.png)
###如何获取user_id
1、打开网址<http://weibo.cn>，搜索我们要找的人，如”郭碧婷“，进入她的主页；<br>
2、大部分情况下，在用户主页的地址栏里就包含了user_id，如”郭碧婷“的地址栏地址为"<http://weibo.cn/u/1729370543?f=search_0>"，其中的"1729370543"就是她的user_id。如图所示：
![](http://7xknyo.com1.z0.glb.clouddn.com/github/weibospider/userid1.png)
但是部分用户设置了个性域名，他们的地址栏地址就变成了"<http://weibo.cn/个性域名?f=search_0>"的形式，如柳岩主页的地址栏地址为"<http://weibo.cn/guangxianliuyan?f=search_0>"。如图所示：
![](http://7xknyo.com1.z0.glb.clouddn.com/github/weibospider/userid2.png)
事实上，如果仅仅爬取微博，用user_id或个性域名都可以，但是因为本脚本还要爬取用户昵称，而用个性域名表示的网页爬取有一些小问题，需要另外的网页。所以，如果遇到地址栏没有user_id的情况，大家可以点击”资料“，跳转到用户资料页面，如柳岩的资料页面地址为"<http://weibo.cn/1644461042/info>"，其中的"1644461042"即为柳岩微博的user_id。如图所示：
![](http://7xknyo.com1.z0.glb.clouddn.com/github/weibospider/userid3.png)

###注意事项
1、user_id不能为爬虫微博的user_id。因为要爬微博信息，必须先登录到某个微博账号，此账号我们姑且称为爬虫微博。爬虫微博访问自己的页面和访问其他用户的页面，得到的网页格式不同，所以无法爬取自己的微博信息；<br>
2、cookie有期限限制，大约两天左右的有效期，超过有效期需重新更新cookie。