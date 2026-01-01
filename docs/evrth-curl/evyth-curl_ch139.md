# IPFS

IPFS 是 *星际文件系统*。curl 仅通过 *HTTP 网关* 支持使用 IPFS。这意味着当提供给它时，它理解 IPFS URL，但你必须同时提供一个有效的网关 URL，以便 curl 可以使用它来检索内容。curl 本身不支持 IPFS。

## 网关

`--ipfs-gateway` 允许用户指定 IPFS HTTP 网关 URL。例如：

```sh
curl --ipfs-gateway http://localhost:8080 ipfs://bafybeigagd5nmnn2iys2f3d/
```

如果你选择使用远程网关，你应该意识到你需要完全信任该网关。在本地网关中这是可以的，因为你自己托管它。在使用远程网关的情况下，可能存在恶意行为者返回与你的请求不匹配的数据，检查或甚至干扰请求。当你使用 curl 获取 IPFS 时，你可能不会注意到这一点。

如果不使用 `--ipfs-gateway` 选项，curl 会检查 `IPFS_GATEWAY` 环境变量以获取指导，如果没有设置，则可以使用 `~/.ipfs/gateway` 文件来识别网关。

IPFS 支持首次添加到 curl 的版本是 8.4.0。
