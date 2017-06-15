# 安卓app打包工具cordova使用教程
![image](https://user-images.githubusercontent.com/18028533/27120577-b9a728d0-5116-11e7-94a2-8cd55266ba04.png)



# 环境搭建
> 材料,准备工作

项目  | 验证安装完成 | 教程
------------ | -------------   | -------------
npm  |  npm -v | node自带
npm install -g cordova |  cordova -v |   [cordova打包教程](http://www.jianshu.com/p/60e98587ae89)
jdk  | javac（安装jdk主要是目录，推荐默认，会安装好jir跟jdk两个目录） |  [jdk安装](http://jingyan.baidu.com/article/bea41d435bc695b4c41be648.html)
sdk | adb这个也是需要配环境变量的 | [sdk环境](http://jingyan.baidu.com/article/f71d603757965b1ab641d12a.html)  [sdk下载](http://tools.android-studio.org/index.php/sdk/)
ant | ant -v | 

# 打包步骤
[cordova打包教程](http://www.jianshu.com/p/60e98587ae89)


```bash
#1. 安装cordova 
npm install -g cordova


# （可选步骤）2.检查环境
$ cordova requirement android


# 3.创建目录结构，文件夹取名为hello
$ cordova create hello


# 4.进入hello文件夹目录
$ cd hello


# 5.创建android平台
$ cordova platform add android --save


```

# 可选热更新步骤

### 一、热更新工具的安装

```bash
# (可选步骤)6.安装热更新插件
cordova plugin add cordova-hot-code-push-plugin


# (可选步骤)7.安装获取mac地址的插件
$ cordova plugin add https://github.com/mohamed-salah/MacAddress.git


# （可选步骤）8.全局安装热更新工具
$ npm install -g cordova-hot-code-push-cli
```

### 二、在静态服务器下创建一个目录。
> 例： /hotcode

### 三 、在项目的根目录下新建一个cordova-hcp.json的文件添加以下内容：
```json
{
  "autogenerated": true,
  "release": "2016.11.08-15.13.26",
  "content_url": "http://10.86.87.112:3033/hotcode",
  "update": "now"
}
```
### 四、生成两个热更新所需文件
```bash
# 使www文件夹下生成两个热更新所需文件chcp.manifest/chcp
$ cordova-hcp build 
```
### 五、早config.xml文件添加热更新配置，并运行构建命令
> 添加热更新配置,此步骤在我自

![image](https://user-images.githubusercontent.com/18028533/27170454-82b21e32-51e0-11e7-84ae-67fb298dc4bf.png)
```html
<chcp>
    <auto-download enabled="false" />
    <auto-install enabled="false" />
    <config-file url="http://10.86.87.112:3033/hotcode/chcp.json"/>
</chcp>
```

> 添加弹窗js代码

```js
//备注：这里的使用了Framework7
chcp.fetchUpdate(function(error, data) {
    if(!error) {
        myApp.modal({
            title: "提示",
            text: "有更新，确定更新吗?",
            buttons: [{
                text: '不更新'
            }, {
                text: "立即更新",
                onClick: function() {
                    myApp.showPreloader('正在升级，升级完毕应用将自动重启...');
                    chcp.installUpdate(function(error) {
                        myApp.alert("更新完成", ["提示"]);
                    })
                }
            }]
        })
    } else {
        myApp.alert("你当前是最新版本", ["提示"]);
    }
})
```

> 运行命令

```bash
$ cordova build android
```

### 六、修改服务端的代码

- 修改www目录下的内容，例如：修改index.html中的代码.
- 然后运行cordova-hcp build.
- 再将index.html和chcp.json和chcp.manifest一起上传到服务器对应的位置。
- 重新打开app将会看见更新的内容。

### 七、弹窗提示
> 将config.xml文件夹中的配置进行修改，注：要在<widget></widget>标签内部，url为改json地址

```html
<chcp>
    <auto-download enabled="false" />
    <auto-install enabled="false" />
    <config-file url="http://10.86.87.112:3033/hotcode/chcp.json"/>
</chcp>
```


# 打包APP流程(注：目录中不能有汉字，不然报错)
```bash

# 构建安卓平台apk
$ cordova build
```


# 输出app
`目录 ： > hello\platforms\android\build\outputs\apk`
