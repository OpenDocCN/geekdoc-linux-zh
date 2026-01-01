# 网站

curl 网站的大部分内容也可在公共 git 仓库中找到，尽管它与源代码仓库分开，因为通常对同一群人来说并不有趣，我们可以维护一个拥有推送权限等的不同人员名单。

网站 git 仓库可在以下 URL 的 GitHub 上找到：[`github.com/curl/curl-www`](https://github.com/curl/curl-www)，您可以通过以下方式克隆网站代码：

```sh
git clone https://github.com/curl/curl-www.git
```

## 构建网站

该网站是一个定制的设置，主要从一组源文件构建静态 HTML 文件。源文件使用一个增强版的 C 预处理器[fcpp](https://daniel.haxx.se/projects/fcpp/)和一组 perl 脚本进行预处理。手册页面通过[roffit](https://daniel.haxx.se/projects/roffit/)转换为 HTML。请确保 fcpp、perl、roffit、make 和 curl 都包含在您的$PATH 中。

第一次克隆 git 仓库后，在源根目录树中运行一次`sh bootstrap.sh`以获取符号链接和一些初始本地文件设置，然后您可以通过在源根目录树中调用`make`来本地构建网站。

注意，这并不使你拥有一个完整的网站镜像，因为一些脚本和文件仅可在实际网站上获取，但它应该足够让你本地查看大多数 HTML 页面。

## 运行本地克隆

网站以易于托管本地副本的方式进行构建，以便在将更改推送到生产中的官方网站之前进行浏览和测试。我们建议您将网站命名为`curl.local`，并将其添加到您本地`/etc/hosts`文件中的一个条目。然后，将您的 HTTP 服务器的文档根指向 curl-www 源代码根目录。

## 网站基础设施

+   公共 curl 网站托管在[curl.se](https://curl.se)。

+   域名归**Daniel Stenberg**所有

+   主要源机器由**Haxx**赞助

+   curl.se 域名由由**Kirei**赞助的 anycast 分布式 DNS 服务器提供服务

+   该网站通过由**Fastly**运行的 CDN 向全球提供服务

+   网站每 N 分钟从 GitHub 更新一次。CDN 前端缓存内容 Y 分钟（不同类型的缓存内容时间不同）
