# BaseUrlManager

[![Download](https://img.shields.io/badge/download-App-blue.svg)](https://raw.githubusercontent.com/jenly1314/BaseUrlManager/master/app/release/app-release.apk)
[![JCenter](https://img.shields.io/badge/JCenter-1.1.1-46C018.svg)](https://bintray.com/beta/#/jenly/maven/base-url-manager)
[![JitPack](https://jitpack.io/v/jenly1314/BaseUrlManager.svg)](https://jitpack.io/#jenly1314/BaseUrlManager)
[![CI](https://travis-ci.org/jenly1314/BaseUrlManager.svg?branch=master)](https://travis-ci.org/jenly1314/BaseUrlManager)
[![CircleCI](https://circleci.com/gh/jenly1314/BaseUrlManager.svg?style=svg)](https://circleci.com/gh/jenly1314/BaseUrlManager)
[![API](https://img.shields.io/badge/API-16%2B-blue.svg?style=flat)](https://android-arsenal.com/api?level=16)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/mit-license.php)
[![Blog](https://img.shields.io/badge/blog-Jenly-9933CC.svg)](https://jenly1314.github.io)
[![QQGroup](https://img.shields.io/badge/QQGroup-20867961-blue.svg)](http://shang.qq.com/wpa/qunwpa?idkey=8fcc6a2f88552ea44b1411582c94fd124f7bb3ec227e2a400dbbfaad3dc2f5ad)

BaseUrlManager for Android 的设计初衷主要用于开发时，有多个环境需要打包APK的场景，通过BaseUrlManager提供的BaseUrl动态设置入口，只需打一
次包，即可轻松随意的切换不同的开发环境或测试环境。在打生产环境包时，关闭BaseUrl动态设置入口即可。

> 妈妈再也不用担心因环境不同需要打多个包的问题，从此告别环境不同要写一堆配置的烦恼，真香。

> 配合[ **RetrofitHelper** ](https://github.com/jenly1314/RetrofitHelper)动态改变BaseUrl一起使用更香。

## Gif 展示
![Image](GIF.gif)

## 引入

### Maven：
```maven
<dependency>
  <groupId>com.king.base</groupId>
  <artifactId>base-url-manager</artifactId>
  <version>1.1.1</version>
  <type>pom</type>
</dependency>
```
### Gradle:
```gradle

//AndroidX 版本
implementation 'com.king.base:base-url-manager:1.1.1'

//-----------------------v1.0.x以前的版本
//AndroidX 版本
implementation 'com.king.base:base-url-manager:1.0.1-androidx'

//Android Support 版本
implementation 'com.king.base:base-url-manager:1.0.1'
```

### Lvy:
```lvy
<dependency org='com.king.base' name='base-url-manager' rev='1.1.1'>
  <artifact name='$AID' ext='pom'></artifact>
</dependency>
```

###### 如果Gradle出现implementation失败的情况，可以在Project的build.gradle里面添加如下：（也可以使用上面的GitPack来implementation）
```gradle
allprojects {
    repositories {
        maven { url 'https://dl.bintray.com/jenly/maven' }
    }
}
```

## 示例

集成步骤代码示例 （示例出自于[app](app)中）

Step.1 在您项目中的AndroidManifest.xml中通过配置meta-data来自定义全局配置
```xml
    <!-- 在你项目中添加注册如下配置 -->
    <activity android:name="com.king.base.baseurlmanager.BaseUrlManagerActivity"
        android:screenOrientation="portrait"
        android:theme="@style/BaseUrlManagerTheme"/>
```

Step.2 在您项目Application的onCreate方法中初始化BaseUrlManager

```java
    //获取BaseUrlManager实例（适用于v1.1.x版本）
    mBaseUrlManager = BaseUrlManager.getInstance();

    //获取BaseUrlManager实例（适用于v1.0.x旧版本）
    mBaseUrlManager = new BaseUrlManager(this);

    //获取baseUrl
    String baseUrl = mBaseUrlManager.getBaseUrl();

```

Step.3 提供动态配置BaseUrl的入口（通过Intent跳转到BaseUrlManagerActivity界面）

v.1.1.x 新版本写法
```JAVA
   BaseUrlManager.getInstance().startBaseUrlManager(this,SET_BASE_URL_REQUEST_CODE);

```

v1.0.x 以前版本写法
```JAVA
    Intent intent = new Intent(this, BaseUrlManagerActivity.class);
    //BaseUrlManager界面的标题
    //intent.putExtra(BaseUrlManagerActivity.KEY_TITLE,"BaseUrl配置");
    //跳转到BaseUrlManagerActivity界面
    startActivityForResult(intent,SET_BASE_URL_REQUEST_CODE);
```

Step.4 当配置改变了baseUrl时，在Activity或Fragment的onActivityResult方法中重新获取baseUrl即可
```java

    //方式1：通过BaseUrlManager获取baseUrl
    String baseUrl = BaseUrlManager.getInstance().getBaseUrl();
    //方式2：通过data直接获取baseUrl
    UrlInfo urlInfo = BaseUrlManager.parseActivityResult(data);
    String baseUrl = urlInfo.getBaseUrl();

```

更多使用详情，请查看[app](app)中的源码使用示例或直接查看[API帮助文档](https://jenly1314.github.io/projects/BaseUrlManager/doc/)
