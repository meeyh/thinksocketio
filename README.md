## 简介

一个使用<a href="https://github.com/walkor/phpsocket.io" target="_blank">PHPSocket.IO</a>，<a href="https://github.com/top-think/framework" target="_blank">ThinkPHP5.1</a>，以及JQ实现的聊天室，包括功能有：

- 公频广场聊天
- 自定义用户名
- 一对一私聊
- 公频中私聊信息提醒
- 新用户加入以及离开提醒
- 在线用户列表，人数统计
- 系统主动推送广播，或向指定用户推送消息

## 截图

广场公频聊天：
<img src="https://res.beltxman.com/20190620105015.png" alt="广场公频聊天">
一对一私聊：
<img src="https://res.beltxman.com/20190620105341.png" alt="一对一私聊">
广播推送系统消息：
<img src="https://res.beltxman.com/20190620105546.png" alt="广播推送系统消息">
指定用户推送消息：
<img src="https://res.beltxman.com/20190620111851.png" alt="指定用户推送消息">

## 使用说明

### 安装

```bash
git clone git@github.com:hsu1943/thinksocketio.git
cd thinksocketio
composer install
```

### 配置

模板文件`index.html`和`chat.html`中`socket`修改为你的`socket`服务端地址。

```bash
var socket = io('http://127.0.0.1:2021');
```

如果需要使用数据库存储聊天记录消息，导入根目录下`socketio.sql`，配置正确的数据库连接。
不用的话需要将源代码中数据库存储语句注释。

### 主动推送系统消息接口

修改配置文件`/config/socketio/app.php`中的配置为监听消息推送地址：

```bash
return [
    'ws' => [
        'apiHost' => 'http://127.0.0.1:2121',
    ],
];
```

该地址即系统推送消息地址，参数：

```html
to：接收人
content：消息
```

两种用法：

1. 其他项目POST，GET请求接口即可推送消息

```html
向username推送系统消息
http://test.com/system?to=username&content=系统推送消息测试
广播消息
http://test.com/system?content=系统推送消息测试
```

`http://test.com/system`这是本项目使用上面的监听地址做的一个消息推送demo，详情看源代码。

2. 本项目中推送系统消息：

   已将推送封装在Msg的模型中，使用：

```php
$res = Msg::send($to, $content);
return $res == 'ok' ? '系统消息推送成功' : '系统消息推送失败';
```

## 测试

运行服务端：

```bash
php ./public/server.php
```

访问以下地址即可进入公频：

```bash
http://test.com/socketio
```

点击消息列表中的用户名即可进入私聊。

## 更新

* 2019-06-19 增加在线人数统计，在线用户列表，修改昵称，添加系统主动推送接口（广播或私信）；

## 开发记录

以下两篇文章是在开发过程中的记录，代码不是最新，最新代码以本项目为准，有问题可以去文章里留言。

[ThinkPHP 5.1+PHPSocket.IO实现websocket搭建聊天室+私聊](https://beltxman.com/archives/2329.html "ThinkPHP 5.1+PHPSocket.IO实现websocket搭建聊天室+私聊")

[ThinkPHP 5.1下使用PHPSocket.IO实现websocket通讯](https://beltxman.com/archives/1885.html "ThinkPHP 5.1下使用PHPSocket.IO实现websocket通讯")

## 感谢

* <a href="https://github.com/top-think/framework" target="_blank">top-think/framework</a>
* <a href="https://github.com/walkor/phpsocket.io" target="_blank">walkor/phpsocket.io</a>
* <a href="https://github.com/walkor/web-msg-sender" target="_blank">walkor/web-msg-sender</a>