
### 功能简介

- 支持GET、POST等请求方式，请求与响应数据自动进行转换处理，无需上层分别定义接口；

- 支持OKHttp本身的Http缓存，也支持外部自定义的在线离线缓存，可配置缓存策略，共有五种缓存策略，如优先获取缓存策略，具体实现参考http包下的strategy包；

- 支持异常统一处理，定制了ApiException拦截处理，统一返回异常信息；

- 支持链式编程，数据通过回调返回，也支持返回Observable，可继续采用RxJava的相关特性；

- 支持失败重试机制，可配置失败重试次数以及重试时间间隔；

- 支持根据Tag中途取消请求，也可以取消所有请求；

- 支持泛型T接收处理响应数据，也可根据服务器返回的统一数据模式定制如包含Code、Data、Message的通用Model ApiResult<T>；由于ApiResult的属性不定，无法做到统一处理，所以单独放到netexpand module中，里面包含与其相关的请求处理，可以根据该module定制属于各自服务器的相关功能；

- 支持全局配置和单个请求的局部配置，如果局部配置与全局配置冲突，那么局部配置会替换全局配置；

- 全局配置支持`CallAdapter.Factory`、`Converter.Factory`、`okhttp3.Call.Factory`、`SSLSocketFactory`、`HostnameVerifier`、`ConnectionPool`、主机URL、请求头、请求参数、代理、拦截器、Cookie、OKHttp缓存、连接超时时间、读写超时时间、失败重试次数、失败重试间隔时间的一系列配置；

- 局部请求配置支持主机URL、请求后缀、请求头、请求参数、拦截器、本地缓存策略、本地缓存时间、本地缓存key、连接超时时间、读写超时时间的一系列配置；

- 支持单文件和多文件上传，上传类型支持图片、字节流、字节数组等，可回调上传进度；

- 支持小文件下载，可回调下载进度；

- 支持内存、磁盘二级缓存以及SharedPreferences缓存，可自由拓展。磁盘缓存支持KEY加密存储，可定制缓存时长；SharedPreferences支持内容安全存储，采用Base64加密解密；

- 支持事件消息发送，采用Rx响应式编程思想建立的RxBus模块，采用注解方式标识事件消耗地，通过遍历查找事件处理方法；

- 支持动态权限申请管理；

- 支持GreenDao数据库操作；

- 支持Glide图片加载。

- ......

### 效果展示

![操作演示动画](https://github.com/xiaoyaoyou1212/XSnow/blob/master/screenshot/screenshot.gif)

### 使用简介

1、引用依赖

在 build 文件的 dependencies 添加如下依赖：
```
compile 'com.vise.xiaoyaoyou:xsnow:x.x.x'
```
其中的 x.x.x 为版本号，下文有具体介绍。

如果需要使用 Glide 加载图片功能，还需要添加如下依赖：
```
compile 'com.github.bumptech.glide:glide:3.7.0'
```

如果需要使用GreenDao数据库功能，还需要添加如下依赖：
```
compile 'org.greenrobot:greendao:3.2.0'
```
并根据 GreenDao 数据库的使用规则进行相关配置。

2、初始化

在 Application 中进行全局初始化以及添加全局相关配置，简易初始化使用如下：

```
ViseHttp.init(this);
ViseHttp.CONFIG()
        //配置请求主机地址
        .baseUrl("服务器地址");
```

如果想快速接入项目中使用只需配置服务器地址就行，其他配置都将采用库中默认的配置。
#### 如果想修改全局相关配置以及查看具体请求调用介绍，请移步 [>>> 详细使用介绍](https://github.com/xiaoyaoyou1212/XSnow/wiki/%E8%AF%A6%E7%BB%86%E4%BD%BF%E7%94%A8%E4%BB%8B%E7%BB%8D)文档。

#### 如果想深入了解项目中用到的相关技术，请移步 [>>> 相关技术说明](https://github.com/xiaoyaoyou1212/XSnow/wiki/%E7%9B%B8%E5%85%B3%E6%8A%80%E6%9C%AF%E8%AF%B4%E6%98%8E)文档。

### 代码托管

[![JCenter](https://img.shields.io/badge/JCenter-2.1.9-orange.svg)](https://jcenter.bintray.com/com/vise/xiaoyaoyou/xsnow/2.1.9/)

相关代码 Release 版本已上传到 JCenter，如需查看相关版本记录可点击上面链接进行查询。

### 版本说明

[![LatestVersion](https://img.shields.io/badge/LatestVersion-2.1.9-orange.svg)](https://github.com/xiaoyaoyou1212/XSnow/blob/master/VERSION.md)

如需查看相关版本记录点击上面进入相关链接查看即可，下面是最新修改记录：

- V2.1.9（2018-06-08）
    - 修改处理数据转换的线程为IO线程；
    - 增加针对Type为String时不解析功能；
    - 修改权限管理持有上下文引用问题。

### 混淆配置
由于 XSnow 库有依赖部分第三方库，所以需要对依赖的第三方库也做相应的混淆保护，具体的混淆配置如下：
```
#glide
-dontwarn com.bumptech.glide.**
-keep public class * implements com.bumptech.glide.module.GlideModule
-keep public enum com.bumptech.glide.load.resource.bitmap.ImageHeaderParser$** {
  **[] $VALUES;
  public *;
}

#gson
-dontwarn com.google.gson.**
-keep class com.google.gson.** { *; }

#rxjava
-dontwarn io.reactivex.**
-keep class io.reactivex.** { *; }

#okhttp
-dontwarn okio.**
-keep class okio.** { *; }
-dontwarn okhttp3.**
-keep class okhttp3.** { *; }

#retrofit
-dontwarn retrofit2.**
-keep class retrofit2.** { *; }
-keepattributes Signature
-keepattributes Exceptions

#greendao
-dontwarn org.greenrobot.greendao.**
-keep class org.greenrobot.greendao.** { *; }
-keepclassmembers class * extends org.greenrobot.greendao.AbstractDao {
    public static java.lang.String TABLENAME;
}
-keep class **$Properties
```

针对 XSnow 库本身的混淆保护配置如下：
```
#XSnow
-dontwarn com.vise.utils.**
-keep class com.vise.xsnow.event.inner.ThreadMode { *; }
-keep class com.vise.xsnow.http.api.ApiService { *; }
-keep class com.vise.xsnow.http.mode.CacheMode
-keep class com.vise.xsnow.http.mode.CacheResult { *; }
-keep class com.vise.xsnow.http.mode.DownProgress { *; }
-keep class com.vise.xsnow.http.strategy.**
-keepclassmembers class * {
    @com.vise.xsnow.event.Subscribe <methods>;
}
-keep class com.bumptech.glide.Glide
```

如果有拷贝使用到拓展库 netexpand，那么需要保护 ApiResult 这个类，具体配置如下：
```
#netexpand
-keep class com.vise.netexpand.mode.ApiResult { *; }
```



