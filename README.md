# 在开源聊天工具中使用chatGPT
<table style="border-collapse: collapse;">
  <tr>
    <td style="border: none;"><img src="https://user-images.githubusercontent.com/130202349/230718521-517d13eb-7cad-41f4-8dbb-e7e7ce3206e1.png" height="200"></td>
    <td style="border: none;"><img src="https://user-images.githubusercontent.com/130202349/230718323-9d70a89a-6830-46ca-8240-9c1df15ca5bd.jpg" height="200"></td>
    <td style="border: none;"><img src="https://user-images.githubusercontent.com/130202349/230718174-0c408dc3-d8fb-4e72-a581-f898b1c06bc9.png" height="200"></td>
  </tr>
</table>

这是通过使用hubot机器人使用chatGPT API来实现在rocketchat中使用chatGPT的方法。

需要使用境外服务器（当然也可以使用境内rocketchat+境外hubot两台服务器），下面以**Debian 11系统**安装为例。

**推荐使用下面的一键脚本安装**

## 1. 运行下面命令
```
curl -sSL https://raw.githubusercontent.com/bytetape/rocketchat_chatGPT/main/installDockerComposeAndRocketchat_debian.sh | sh
```
<img src="https://user-images.githubusercontent.com/130202349/230774254-82cfe3af-8375-42ae-b9a7-4a4f16ed5625.jpg" alt="1" height="200">

## 2. 配置rocketchat
1. 使用**http://[你的服务器IP]:3000**进入rocketchat界面（可能要等待刷新几十秒）。

2. 完成step1-3的**Admin Info**信息维护，中间弹出的警告点“是”。完成信息维护后记得回到邮箱点Verify registration！

3. 回到rocketchat创建一个机器人，命名为“chatGPT”, 密码chatGPT，这里名字和密码和.env文件里的配置要保持一致，Roles选择bot。
<img src="https://user-images.githubusercontent.com/130202349/230774208-63aa933c-a86d-4c2d-a40f-a01e0a1b9bff.jpg" alt="1" height="200">

## 3. 运行下面命令
```
curl -sSL https://raw.githubusercontent.com/bytetape/rocketchat_chatGPT/main/installHubotWithChatGPT_debian.sh | sh
```
<img src="https://user-images.githubusercontent.com/130202349/230774175-26330460-ee6a-485b-958f-2b45c4f30ca5.jpg" alt="1" height="200">

运行下面代码修改openAI_API
```
# 替换 API 密钥
cd /opt/rocketchat_chatGPT
read -p "请输入 openAI API:" API && \
sudo sed -i "s/YOUR API KEY/$API/g" /opt/rocketchat_chatGPT/scripts/script_chatGPT.js
```
启动机器人
```
# 启动机器人
source /opt/rocketchat_chatGPT/.env
cd /opt/rocketchat_chatGPT && chmod +x bin/hubot && source .env && bin/hubot -a rocketchat
```
<img src="https://user-images.githubusercontent.com/130202349/230774136-98ded94f-517a-4421-a719-1efd6cbaf861.jpg" alt="1" height="200">

---

需要注意的是如果我们关闭了SSH客户端，此时正在运行的进程也会中断。解决方法就是把进程转入后台，操作如下：

先使用Ctrl+C终止进程，输入下面命令
```
source .env;bin/hubot -a rocketchat & disown -h
```
后续如果需要关闭进程可用下面命令找到后台进程，并kill
```
ps -ef | grep hubot
kill <进程号>
```
