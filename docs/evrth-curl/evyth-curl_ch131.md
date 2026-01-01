# 客户端证书

TLS 客户端证书是客户端通过密码学方式向服务器证明其确实是正确对等方的一种方式（有时也称为双向 TLS 或 mTLS）。使用客户端证书的命令行指定了证书及其相应的密钥，然后它们在 TLS 握手过程中传递给服务器。

在进行此操作时，您需要将客户端证书事先存储在文件中，并且您应该已经通过不同的渠道从正确的实例中获取了它。

密钥通常由密码保护，您需要提供或交互式地提示以获取。

curl 提供了选项，允许您使用`--cert`指定一个既是客户端证书又是私钥的单一文件，或者您可以使用`--key`独立指定密钥文件：

```sh
curl --cert mycert:mypassword https://example.com
curl --cert mycert:mypassword --key mykey https://example.com
```

对于某些 TLS 后端，您也可以使用不同的类型传递密钥和证书：

```sh
curl --cert mycert:mypassword --cert-type PEM \
     --key mykey --key-type PEM https://example.com
```
