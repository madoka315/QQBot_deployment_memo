# QQ机器人部署备忘
## 2026.9
增加部署方式的可选项:

- 基于NTQQ协议端SnowLuma

此阶段考虑NapCatQQ或LLOnebot(已命名为LuckyLilliaBot)或SnowLuma方式。
## 2024.5
相比2023.11，增加一些部署方式的可选项。
目前部署qq机器人方式总结为：

- 基于Andorid官方端+LSPosed框架的openshamrock 
- 基于Desktop官方端+LiteloaderQQNT框架的LLOnebot 或 Chronocat(v0.2后仅支持Satori协议)
- 基于无头客户端的NapCatQQ
- 基于官方平台api的gensokyo
- 基于NTQQ协议端Lagrange.OneBot

并且，LiteloaderQQNT与Chronocat当前恢复正常维护，原备忘中**安装Liteloader**与**安装Chronocat**的部分应遵循最新官方文档。

由于Chronocatv0.2后取消支持Red协议，为方便部署，此阶段考虑NapCatQQ或LLOnebot方式。它们均直接支持反向ws，因此可不需要**安装TRSS-Yunzai与ws-plugin**，只是因为yobot的特殊性仍需要ws-plugin作中间通信。
## 2023.11
注意：具有时效性，本篇仅为备忘，部署的是**自用**目的的公主连结机器人。写于2023/11/16。提到的文件均位于release。

由于qsign遭到围堵，使用gocq有很大概率出现冻结或永久风控。现在部署qq机器人仅考虑 QQNT+Liteloader（使用Chronocat插件作为服务端，最终版本仍然可以使用）或 Android QQNT+shamrock或 官方QQ机器人的方式。本篇记录采用第一种方式在Windows上成功部署并同时运行yunzai& hoshino& yobot的过程。
本次部署使用目前最新可用Liteloader的QQNT（16183版本）以便使用Chronocat。

**第一步：安装QQNT。**

准备好文件后，安装`QQ9.9.2.16183_x64.exe`，然后成功登录一次并退出。

**第二步：安装Liteloader。**

解压`Liteloader.zip`，打开QQNT安装目录，将LiteLoader文件夹放入 \resources\app 文件夹内。然后任意位置打开`LiteLoaderPatchNFixer.exe`，进行修补。如QQ程序自动运行，需要退出。

**第三步：安装Chronocat插件。**

解压`LiteLoaderQQNT-Plugin-Chronocat-master.zip`，将LiteLoaderQQNT-Plugin-Chronocat-master文件夹放入 当前用户目录\Documents\LiteLoaderQQNT\plugins 文件夹内。启动QQNT，确保 设置-LiteLoader 配置界面-扩展 中Chronocat已安装并已启动。

**第四步：安装TRSS-Yunzai与ws-plugin。**

参考官方库即可。由于需要RedProtocol，ws-plugin的安装指令需要改为：
```bash
#gitee
git clone --depth=1 -b red https://gitee.com/xiaoye12123/ws-plugin.git ./plugins/ws-plugin/
pnpm install --filter=ws-plugin
```

**第五步：配置ws-plugin以便连接qqnt和其他bot。**

在 当前用户目录/.chronocat/config/chronocat.yml 中可以找到Chronocat的red协议端口（默认端口16530）和token，将其记住。然后打开 Yunzai目录/plugins/ws-plugin/config/config/ws-config.yaml，新增如下配置并保存:
```yaml
 - name: qqnt
    address: IP:red协议端口
    accessToken: '上述token'
    type: 4
    reconnectInterval: 5
    maxReconnectAttempts: 0
    uin: bot号码
```
此时Yunzai可以正常获取QQNT的消息。如果要配置其他bot，以下是HoshinoBot的示例：
```yaml
 - name: hoshino
    address: ws://127.0.0.1:8080/ws/
    type: 1
    reconnectInterval: 5
    maxReconnectAttempts: 0
    uin: bot号码
```

# 官方仓库地址：
- unidbg-fetch-qsign 已删
- go-cqhttp：[https://github.com/Mrs4s/go-cqhttp](https://github.com/Mrs4s/go-cqhttp)
- LiteloaderQQNT：[https://github.com/LiteLoaderQQNT/LiteLoaderQQNT](https://github.com/LiteLoaderQQNT/LiteLoaderQQNT) 
- Chronocat：[https://github.com/chrononeko/chronocat](https://github.com/chrononeko/chronocat)
- LLOneBot/LuckyLilliaBot: [https://github.com/LLOneBot/LuckyLilliaBot](https://github.com/LLOneBot/LuckyLilliaBot)
- NapCatQQ: [https://github.com/NapNeko/NapCatQQ](https://github.com/NapNeko/NapCatQQ)
- OpenShamrock：[https://github.com/whitechi73/OpenShamrock](https://github.com/whitechi73/OpenShamrock)
- gensokyo: [https://github.com/hoshinonyaruko/gensokyo](https://github.com/hoshinonyaruko/gensokyo)
- Lagrange.Core: [https://github.com/KonataDev/Lagrange.Core/](https://github.com/KonataDev/Lagrange.Core/)
- TRSS-Yunzai：[https://github.com/TimeRainStarSky/Yunzai](https://github.com/TimeRainStarSky/Yunzai)
- ws-plugin：[https://github.com/xiaoye12123/ws-plugin](https://github.com/xiaoye12123/ws-plugin)
- HoshinoBot：[https://github.com/Ice9Coffee/HoshinoBot](https://github.com/Ice9Coffee/HoshinoBot)
- yobot：[https://github.com/yuudi/yobot](https://github.com/yuudi/yobot)
- SnowLuma: [https://github.com/SnowLuma/SnowLuma](https://github.com/SnowLuma/SnowLuma)
