# 获取NIP-05认证的中文教程

NIP-05是一项Nostr改进提案，概述了将Nostr密钥映射到基于DNS的互联网标识的过程。这允许用户将他们的Nostr公钥与易读写的互联网地址关联起来，如 “gg@gejiliang.gg”从而让户更容易在Nostr上找到和跟踪他人，以及在Nostr客户端上可以更用户友好地显示用户标识符。

目前已经有不少供应商提供免费和收费的NIP-05认证服务，这里选取有代表性的三个：nost.vip、getalby和nostrplebs为例。如果这些满足不了你可以跳转到最后自建认证的部分。

## 使用nost.vip

最简单的是nost.vip这一类的，你只需要将 npubxxxx@nost.vip (npubxxxx为你的公钥) 填写到自己的Profile中NIP-05信息栏中点击保存即可，如下图以 Damus 为例：

![NIP-05设置栏](https://nostr.build/i/nostr.build_776d25e68b4f6a47722dfd745f845ba9a3c290296b8b975ced36096bfe66bf5a.png)

需要说明的是，此种直接填写公钥的方法并不是所有客户端都支持，如Snort里就不行，Damus和iris.to是可以的。

## 使用getalby

如果你习惯在电脑上使用浏览器客户端，推荐使用类似getalby.com提供浏览器插件钱包的供应商，__网络打赏和乞讨的体验更丝滑__，而且可以同时获得闪电钱包地址和认证，以下步骤以getalby为例：

1. 首先打开https://getalby.com 点击右上角的 Install Alby 安装浏览器插件；

2. 安装完成后，首先设置解锁密码，然后选择sign up 创建帐号，输入邮箱和登陆密码，这里注意可选生成一个[xxx@getalby.com](mailto:xxx@getalby.com) 的闪电网络地址；

![Untitled](https://nostr.build/i/nostr.build_40781a8d810c7fc16b3fd5e44647422120ee17b13c26f1bda1c1e0ac4e0f6d45.png)

3. 打开 https://getalby.com/settings 选择Profile settings，填上你的**Nostr public key并保存；**

![Untitled](https://nostr.build/i/nostr.build_006f251663b8b439fdcb37124b8406e67d929b0a1bcd06ab7d60d136da455a2a.png)

4. 最后在任意 Nostr 客户端的Profile设置里的NIP-05设置里填上第二步得到的邮箱地址就可以了。

![Untitled](https://nostr.build/i/nostr.build_d6a08e712e6535552ee994627a4b211eedace448318fa4309c5260675909aa02.png)

同时可以在闪电钱包tips也填上这个地址，在支持的客户端里你的个人页面里就会出现闪电标志，别人点击就可以直接给你打钱而不再需要每次都生成收据了。

![Untitled](https://nostr.build/i/nostr.build_60dde8c5f40205ff2208d9e7b7bffbe878d4509e124ca3bf06d9e6439f5e48b7.png)

## 使用Snort和Nostrplebs

如果你使用的是Snort客户端，并且想有一些看起来很fancy的认证标签（类似下图带渐变色的nostr.fan），那么在Snort中的Profile设定里点击NIP-05栏旁边的Buy按钮即可跳转到付费购买页面。详情可以在[https://nostrplebs.com/](https://nostrplebs.com/) 查看，不同位数的价格不同，2字符65k聪，越长越便宜，付费是一次性的，也可以更改用户名对应的公钥。其他设置大同小异，这里不再赘述。

![Untitled](https://nostr.build/i/nostr.build_a084a9a3c631f043f0259d4182bce2d3183f9134920efd9077edd7f6a9c32800.png)

## 用自己的域名进行NIP-05认证

从这里开始你需要具备一点点技术基础，如果没有现学也不会花费你太多时间。下面是写给新手看的，大佬们直接看这里：[https://github.com/nostr-protocol/nips/blob/master/05.md](https://github.com/nostr-protocol/nips/blob/master/05.md)

Github Pages是最简单的方式，你只需要有github帐号和1个域名就行，当然也可以在自己的服务器或vps上部署。

### 1 设置你的域名记录

为你的域名添加一条CNAME记录，记录值为 {你的用户名}.github.io 。这里可以使用根域名，也可以使用子域名。

### 2 创建你的GitHub仓库

建议专门为此目的创建一个新的Github仓库。创建名为".well-known/nostr.json"的新文件。这个文件将保存你的Nostr公钥与你的互联网标识符的映射关系，它是NIP5验证过程中的一个重要组成部分。

nostr.json中保存的是你的公钥的十六进制格式，以及你想要的标识符的昵称。如果你的公钥是 "npub1... "的格式，你可以用[Damus的工具](http://damus.io/key)将其转换成十六进制。你可以用我的NIP-05作为一个模板。

```bash
{
  "names": {
    "gg": "c933319100dd1c4dd9cbb40b7fe31d386c5de5b1cd889f6aaac53a7623b48a12"
  }
}
```

只需用你自己的十六进制公钥替换上面的公钥，并根据需要调整昵称。

最后，在版本库的根目录下创建一个新文件，名为"_config.yml"。这个文件应该包含一行。

```bash
include: [".well-known"]
```

这一行告诉Github，当你的版本库作为静态网站提供时，要包括".known "目录，确保你的NIP5验证文件可以被访问，并且格式正确。

### 3 部署你的版本库

导航到你的版本库的”Setting”页面，选择 "Pages "标签。

在 "Build and deployment "部分，选择 main分支。这将告诉 Github 使用 这个分支中的文件将版本库构建为静态网站并提供服务。

接下来，在 "Custom domain "下，键入你的域名，根域名例如 "gejiliang.gg"或者子域名”nostr.gejiliang.gg”都可以，只要和你在第一步中的设置对应就可以。Github可能会提供一个警告，但你可以安全地忽略它，等待github完成DNS检查就可以了，如果check失败一般是域名的DNS设置或者域名服务商的配置问题。

最后，确保选择 "Enforce HTTPS "选项。这将确保你的网站是通过安全连接提供的，为你的NIP5验证文件提供额外的保护和安全。

可以通过浏览器访问 [https://gejiliang.gg/.well-known/nostr.json](https://xxx.com/.well-known/nostr.json) (gejiliang.gg替换成你自己的域名）来验证是否配置成功。

最后，就可以把自己的NIP-05标识填写到Nostr的客户端里面去了。

