# MQTT

简单的 GET 订阅主题并打印所有发布的消息。执行 POST 将发布数据发布到主题并退出。

订阅由 example.com 发布的 `home/bedroom` 主题的温度：

```sh
curl mqtt://example.com/home/bedroom/temp
```

将值 `75` 发送到由 example.com 服务器托管的 `home/bedroom/dimmer` 主题：

```sh
curl -d 75 mqtt://example.com/home/bedroom/dimmer
```

## curl 对订阅的响应是什么

它输出两个字节的主题长度（MSB | LSB），然后是主题和有效载荷。

## 注意事项

截至 2022 年 9 月 curl 的 MQTT 支持的剩余限制：

+   仅实现了 QoS 级别 0 的发布

+   无法设置发布保留标志

+   不支持 TLS (mqtts)
