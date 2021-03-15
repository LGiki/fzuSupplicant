# fzuSupplicant

[![GitHub license](https://img.shields.io/github/license/LGiki/fzuSupplicant?style=flat-square)](https://github.com/LGiki/fzuSupplicant/blob/master/LICENSE)

这个项目试图将集美大学的第三方锐捷认证客户端[jmuSupplicant](https://github.com/ShanQincheng/jmuSupplicant/)修改为适用于福州大学的第三方锐捷认证客户端，仍在开发中，目前的进展是服务端提示“密码错误”，猜测是由于Response, Identity包或Response, MD5-Challenge EAP包的填充位置存在错误。最近事情比较多，开发缓慢，欢迎Pull Request。

目前福州大学可行的路由器解决方案是使用[mentohust-v4-proxy](https://github.com/updateing/mentohust-v4-proxy)代理发送心跳，需要使用锐捷客户端协助发送认证包，后续发送心跳包维持连接由[mentohust-v4-proxy](https://github.com/updateing/mentohust-v4-proxy)完成，详细过程可以查看其[README文档](https://github.com/updateing/mentohust-v4-proxy/blob/master/README.md)，目前已测试能稳定运行。

关于第三方锐捷认证客户端的实现过程，可以参考[锐捷认证过程分析与第三方锐捷认证客户端的设计与实现](https://github.com/ShanQincheng/jmuSupplicant/blob/master/doc/%E9%94%90%E6%8D%B7%E8%AE%A4%E8%AF%81%E8%BF%87%E7%A8%8B%E5%88%86%E6%9E%90%E4%B8%8E%E7%AC%AC%E4%B8%89%E6%96%B9%E9%94%90%E6%8D%B7%E8%AE%A4%E8%AF%81%E5%AE%A2%E6%88%B7%E7%AB%AF%E7%9A%84%E8%AE%BE%E8%AE%A1%E4%B8%8E%E5%AE%9E%E7%8E%B0.pdf)。

# 编译说明

详细编译说明请查看[Docs/Compile.md](./Docs/Compile.md)。

# 使用说明

编译完成后使用以下指令进行锐捷认证：

```bash
sudo ./fzuSupplicant -u学号 -p密码 -s0
```

程序输出锐捷认证信息，或显示login success，则表示认证成功。

可以通过```--help```参数来获取程序运行帮助。

# License

Apache Version 2.0
