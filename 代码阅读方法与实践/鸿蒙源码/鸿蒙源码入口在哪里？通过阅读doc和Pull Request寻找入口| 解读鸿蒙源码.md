

# 鸿蒙源码入口在哪里？通过阅读doc和Pull Request寻找入口| 解读鸿蒙源码

## 寻找鸿蒙源码入口

网上说鸿蒙操作系统开源了，全部134个仓库[OpenHarmony \- Open\_Harmony \- Gitee\.com](https://gitee.com/organizations/openharmony/projects)，啥？134个仓库是什么东西？不应该是1个仓库吗？Linux源码好像就一个压缩吧，为什么鸿蒙冒出134个仓库来，然后看那些仓库名，很多hi35xx，难道是海思摄像头操作系统？不应该是桌面操作系统吗？

还是上网搜吧，因为鸿蒙源码刚开源，所以网上是不会搜到源码讲解的，搜到的基本都是鸿蒙OS应用开发教程，这个和鸿蒙源码是两回事哈。

搜到一个官方开发教程，[创建一个新的工程](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/create_new_project-0000001053342414)，看完hello world后，发现这个和鸿蒙源码是两回事，我理解是这个开发的应用运行在华为设备操作系统上，开源的就是这些设备上操作系统。

所以得到如下的**理解**：

华为硬件设备->鸿蒙操作系统->SDK->HUAWEI DevEco Studio基于SDK二次开发->APP->运行在鸿蒙操作系统->程序控制华为硬件设备工作，所以那134仓库估计就是操作系统五花八门的功能组件了。

SDK 官网有个[术语](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/glossary-0000000000029587)页面，这是个突破口，因为SDK最终是和操作系统打交道的，所以SDK必然调用的是操作系统的接口，所以这里的术语也差不多是操作系统提供的核心业务接口了。

所以现在就是要在源码里找到这些**术语**，怎么找？当然是把这些仓库全部下载下来全局搜下，网上已经有人共享了，这里分享下：[\(鸿蒙2\.0完整源码（截止200916，134个仓库）](https://blog.csdn.net/myembedded/article/details/108631911)

![image-20200917222032887](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200917222032887.png)

这时再回到官方的鸿蒙仓库[OpenHarmony \- Open\_Harmony \- Gitee\.com](https://gitee.com/organizations/openharmony/projects)，发现好理解了：

![image-20200917215326693](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200917215326693.png)

仓库还是很多的，重点关注的是文档仓库，doc仓库就在第1页上面，如果**仔细浏览**一下第1页也能找到[docs: OpenHarmony开发者文档](https://gitee.com/openharmony/docs)。

这里看到一个现象，就是PR已经有154个合并了，就是差不多至少有几十人多人已经在提交代码了，不知道他们是谁，感兴趣的可以观察一下PR提交者的主页哈，看看有没有什么项目。

所以，**入口**就是看别人已经被合并的PR。

## doc

[docs: OpenHarmony开发者文档](https://gitee.com/openharmony/docs)

开发者文档是很重要的资料，有必要把整个仓库都浏览一遍，这里浏览后手动绘制了一个思维导图，大家可以把前面的总仓库下载下来，然后在vscode里用Go Live功能在浏览器里看。















文章末尾超链接注明：本文参与了[「解读鸿蒙源码」](https://my.oschina.net/gitosc/blog/4559092)技术征文，欢迎正在阅读的你也加入。

#鸿蒙专区 #鸿蒙源码