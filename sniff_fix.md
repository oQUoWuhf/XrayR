## 修复 panic: runtime error: slice bounds out of range \[:XXXX] with capacity 2048

这个错误是xray-core内核的问题，观察issue后发现，该问题在v2ray-core内核上也会同时发生。

因为XrayR使用的xray-core已经落后最新版本太多，所以可能无法直接升级到新版xray-core（需要花额外的时间修改代码以兼容最新内核版本），此处提供一个应急的修复方案。

特别鸣谢 Vigilans 在 v2ray-core 中提交的[pull request](https://github.com/Vigilans/v2ray-core/blob/3eacb0a8d72275ba0890cd73ddf3f9d143015e2e/common/protocol/quic/sniff.go)。

根据他修改的内容，我也试着把修改的内容合并到 xray-core 的 v1.8.20 版本中，经过初步验证，XrayR已无重启现象。

以下内容将说明如何编译XrayR并替换。

### 安装 go 1.23

安装go的步骤省略。网上教程很多。

### 拉取个人修复后的 xray-core
```shell
git clone https://github.com/oQUoWuhf/Xray-core
```

### 拉取我指定本地依赖的 XrayR
```shell
git clone https://github.com/oQUoWuhf/XrayR
```

### 编译 XrayR
参考[XrayR文档](https://xrayr-project.github.io/XrayR-doc/xrayr-xia-zai-he-an-zhuang/install/manual.html)

```shell
cd XrayR/ && go mod tidy && go build -o XrayR -ldflags "-s -w"
```

### 替换 XrayR
```shell
chmod +x XrayR && cp XrayR /usr/local/XrayR/XrayR
```

### 重启 XrayR
```shell
XrayR restart
```

最后观察log，重启的问题是否解决。
