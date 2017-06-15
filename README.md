# ProjectRepost
阶段性项目总结

1，引入魔窗SDK
使用pod方式引入SDK，pod 'MagicWindowSDK'；
2，SDK方法使用
魔窗官方文档案例直接在AppDelegate的实现文件中接入相关的方法，我建议封装一个单独的Class类来管理魔窗SDK的方法，在AppDelegate中使用自定义的Class类；
3，项目设置项
1)	在Xcode项目中TARGETS->Info->URL types中添加针对魔窗SDK处理的scheme；
2)	在苹果开发者账号中开启Associated Domains，重新下载开发者证书；
3)	在Xcode项目中TARGETS->Capabilities开启Associated Domains，在Associated Domains添加魔窗上返回域名，如下配置：
4)	在AppDelegate中调用封装魔窗SDK的方法，自定义类中主要功能如下：
a)	根据魔窗后台AppKey调用接口注册App[MWApi registerApp:AMG_MAGIC_WINDOW_APPKEY];
b)	测试开发阶段开启记录打印[MWApi setLogEnable:YES];
c)	根据魔窗后台配置的mlink key注册从点击短链进入App的回调；
d)	如果点击的短链在源码中没有匹配mlink key注册的回调，则会调用默认回调；
4，页面跳转
在魔窗后台配置好跳转路径，路径配置如下：

1)	服务名称是用于标识这条短链的标志
2)	服务key是用于标识从H5页面进入App内所要到达目的路径，App中用于注册回调；
3)	URI是H5页面传递参数到App的格式，开始时应协商好；
4)	App中在对应注册的服务key的回调中可以获取到从H5页面传递过来的参数，根据实际情况做页面的跳转：
a)	已安装App，获取到参数，在App启动完成并进入主页面后执行相应页面跳转；
b)	未安装App，从短链进入到下载完App，启动App，获取到参数并把参数缓存，等待App进入到主页面才执行相应页面跳转，而且将缓存参数清除；
5，魔窗位使用
魔窗位官方介绍是不同App间数据传递的场景使用，但是我们目前是在本身App内配合活动模板一起使用；
1)	一个App可使用使用多个魔窗位，有唯一的key对应，一个魔窗位只能被定向一个可用的活动；
2)	活动模板是在魔窗位上展示的具体内容，一个活动模板可以定向到不同的魔窗位上；
3)	一个魔窗位不能使用到不同平台，必须分别创建iOS、android平台上的魔窗位，将创建好的活动模版定向到两个魔窗位上；
6，使用过程中遇到的问题与解决方案
1)	pod SDK时，如果项目中已经引入微信SDK，可以使用pod 'MagicWindowSDK-noWXSDK', :podspec => "https://raw.githubusercontent.com/ran354101066/mw-iossdk/master/MagicWindowSDK-noWXSDK.podspec"；
2)	如果首次安装App时，网络不佳，有可能会不能从短链唤起App，请尝试卸载App重新安装再试；
3)	如果希望魔窗后台记录不同渠道的信息，在短链后拼接mw_ck=渠道名，这样就可以在魔窗后台看到对应渠道的相关用户行为信息；
4)	如果从短链可以唤起App，但是无法获取到参数，估计是key或者魔窗服务器慢的原因导致的；

