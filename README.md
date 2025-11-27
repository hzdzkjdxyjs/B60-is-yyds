# B60-is-yyds
如何在B60开发部署和运维
---
## 首先非常抱歉的告诉大家，这款软件并不能开源，因为这是我实习期间给公司做的软件，所以有需要可以联系我们公司的销售
## 但是这款软件很nb，可以说能够完成AI全流程开发
<img width="1512" height="982" alt="截屏2025-11-27 10 31 49" src="https://github.com/user-attachments/assets/3962b724-8683-4c9c-b850-8735591832d7" />

---
## 模型启动
 - 我第一次实习的公司，如果要启动模型是选择一个叫xinference的软件，通过图形化的界面来启动模型，但是这有两个问题
 - 一个是你可能不知道模型启动到底为什么失败
 - 一个是你不能监控模型和GPU的情况
 - 以上两个问题都可以在我的软件解决，我先讲我是怎么实现模型启动的
 - 你可以根据任务类型直接选择你要启动的模型
 - 忘了强调一点，您可以在启动界面决定是否开启和关闭思维链
<img width="1512" height="982" alt="截屏2025-11-27 09 38 26" src="https://github.com/user-attachments/assets/a589c0bf-0342-4c55-a283-4a4a5bb0e451" />
 
 - 再通过鼠标点击选择GPU，这是不是很酷，这里有个设置允许你刷新GPU，目的是你可以直观的看到GPU的显存占用情况，从而更好的选择GPU
<img width="1512" height="982" alt="截屏2025-11-27 09 38 46" src="https://github.com/user-attachments/assets/5a2b0dab-5396-4826-b8c8-2620d05c7042" />
 
 - 最后你可以选择一些较为高级的参数，并启动模型
<img width="1512" height="982" alt="截屏2025-11-27 09 38 53" src="https://github.com/user-attachments/assets/cf4c037a-1f6f-42a4-9469-9cd9d30cfade" />
 
 - 启动成功，你可以直接通过api的形式调用模型，通过点击就可以复制地址了，注意，这里很贴心的为您准备了网卡设置，您可以在启动的.env文件中指定调用模型的网卡的地址，也可以在前端右上角的调用主机，通过人为的输入调用模型的api地址，一般用户可以忽略这一点

<img width="1512" height="982" alt="截屏2025-11-27 09 39 03" src="https://github.com/user-attachments/assets/19477ef3-bd87-46ca-aa7e-d136e531604a" />

 - 对于启动失败，我们也可以通过阅读日志的方式得知启动失败的原因，运维人员就可以根据日志调整参数重新启动模型

<img width="1512" height="982" alt="截屏2025-11-27 09 39 19" src="https://github.com/user-attachments/assets/147bfd77-1ce1-41d9-bf4e-e6a29efbf768" />

 - 对于启动模型，我们在高级参数中提供在线量化和离线量化两种模式，您可以在下载的时候直接选择量化模型如awq模型（离线量化），也可也选择原版模型通过添加量化参数进行量化，目前只允许您使用int4和fp8两种量化模式，主要也是这两种量化方式比较优秀。

<img width="1512" height="982" alt="截屏2025-11-27 09 51 58" src="https://github.com/user-attachments/assets/2c46d709-0731-4057-bc0d-361e7dbd0dd4" />
<img width="1512" height="982" alt="截屏2025-11-27 09 52 14" src="https://github.com/user-attachments/assets/32b9bc01-74fb-4796-b2ba-cd18678e302a" />
---
## 模型监控
 - 老实说模型监控其实有两部分组成
 - 一部分是模型性能监控，您可以通过类似面板的前端界面监控模型
 - 另外一部分是模型存活状态监控，您可以看到我们的模型其实有三种状态分别是启动中，运行中和运行失败获或者结束运行
 - 这是参考了xinference的心跳进程设计的一种轮询机制，来判断模型是否存活
---
 - 您可以看到我有三种模型的指标可以展示，分别是事实数据，折线图和热力图
 - 其次是折线图和热力图属于实时数据，您可以自定义选择数据段时间展示数据，其次是展示的数据步长是多少，比如一个小时的数据，那到底返回这个时间几个点的数据
 - 刷新率是指实时数据多久刷新一次
<img width="1512" height="982" alt="截屏2025-11-27 09 57 18" src="https://github.com/user-attachments/assets/2464646b-14f9-4019-85d7-121d3535c2f3" />
 - 其次是我们的panel可以通过移动拖拽，来决定展示的顺序，通过点击指标偏好可以决定要显示哪些数据（别忘了保存结果）

https://github.com/user-attachments/assets/d804ea28-6242-4d2a-af96-3447703fcf2e

---

## GPU硬件监控
 - 不少做服务器的小伙伴都知道BMC这个概念，GPU监控部分我对GPU的6个传感器做了对应的监控，这也就意味着您可以通过事件面板了解GPU到底发生了什么

https://github.com/user-attachments/assets/a3098ab9-f08b-4c0b-837c-cc680ce98b8f

 - 除了BMC这些概念，还有一些概念比如GPU的实时监控
 - 这里很多的设定和模型监控是一样的，比如支持勾选来展示数据，支持移动折线图和panel来选择展示数据（不要忘了保存偏好数据）
<img width="1512" height="982" alt="截屏2025-11-27 10 20 03" src="https://github.com/user-attachments/assets/2f74378b-55f8-4c84-8dfc-7ac799bde12a" />
<img width="1512" height="982" alt="截屏2025-11-27 10 19 55" src="https://github.com/user-attachments/assets/43573c37-f5b7-48c3-8288-0e0b9f97d305" />
<img width="1512" height="982" alt="截屏2025-11-27 10 19 48" src="https://github.com/user-attachments/assets/e81e56b9-60df-4283-9085-c005f8173b2d" />

---

## 模型商店
 - 其实国内很多时候下载模型会遇到各种问题，比如镜像源不对，网络太烂之类的，那我们提供一个类似xinference的下载模式
 - 但不同的是，您可以选择在我们的界面进行网速测试，系统会自动给您选择最快的镜像，您就可以下载模型了（国内推荐魔搭）
 - 同时我们支持断点续传和本地模型缓存文件删除，如果您的硬盘不够用了，可以在这里删除缓存哦
<img width="1512" height="982" alt="截屏2025-11-27 10 27 54" src="https://github.com/user-attachments/assets/4e620ff8-26d1-4979-a27f-c2b6139bf8a0" />
<img width="1512" height="982" alt="截屏2025-11-27 10 28 00" src="https://github.com/user-attachments/assets/fbea1a21-e084-48df-8c31-1e4d875a1e6c" />
<img width="1512" height="982" alt="截屏2025-11-27 10 27 30" src="https://github.com/user-attachments/assets/1e8d82b0-3adf-461d-9949-7aff70201ecf" />

---

## 应用管理中心
 - 应用管理中心的功能主要是增减减少前端app的数量，改变app的顺序等

https://github.com/user-attachments/assets/474c9540-cba1-446b-a127-08b30f4af625

https://github.com/user-attachments/assets/bab7595c-5a1d-4967-a3a6-9de34fcfb6e1

## AI运维中心


https://github.com/user-attachments/assets/0eb1740b-7574-4f89-96eb-eca6b017929a


## Miner-U文档解析

https://github.com/user-attachments/assets/9ba648f4-6735-4496-b787-37fdbe66c480









