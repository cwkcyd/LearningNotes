### android 进程优先级

android系统中，有如下四种进程，他们的优先级排序如下：

1. 前台进程
2. 可见进程
3. 服务进程
4. 缓存进程

#### 前台进程

前台进程，很难被系统销毁掉，有如下几种情况会被视为前台进程：

- 有activity运行了onResume,在前台和用户交互。
- 有broadcastReceiver 当前正在运行 onReceive方法。
- 有service正在运行onCreate，onStart，或者onDestroy方法。


#### 可见进程

可见进程被回收的几率也不大，只有在需要保证前台进程正常运行不得不回收的时候，才会回收可见进程，以下情况，系统判定为可见进程：

- 有Activity执行了onPause方法，在paunsed状态下运行。
- 有service，通过调用 Service.startForeground()来运行
- 有系统正在使用的服务，例如动态壁纸，输入法等。

#### 服务进程

服务进程，并不可见，但是由于有可能在做一些重要任务，比如上传下载等，所以一般也不会关闭它，但是如果服务进程运行**时间过长**（比如30分钟或者更多），那么它将会被降级为缓存进程（下一个等级）。

服务进程的认定：

- 有服务以startService启动并运行。

#### 缓存进程

缓存进程当前并不被需要，系统缓存他们，是为了更快的切换应用。一般来说，他们被缓存在一个伪LRU的队列中，队列的实现依赖与具体平台，但是一般来说，会试图保存更加有用的进程（例如桌面应用或者用户看见的上一个activity）。同时，缓存进程的销毁还遵守其他的规则：例如，缓存进程存在的数量上限，时间上限等。

----------


注意：进程的优先级不仅仅取决于自身组件的优先级，也受到其他App的影响。

例如A应用正在使用B应用的Contentprovider，那么B应用的优先级**至少**会和A一样高



