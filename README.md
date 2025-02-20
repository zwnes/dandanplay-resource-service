# dandanplay-resource-service

![GitHub release](https://img.shields.io/github/v/release/LussacZheng/dandanplay-resource-service?include_prereleases&label=version&color=important)
[![GitHub All Releases](https://img.shields.io/github/downloads/LussacZheng/dandanplay-resource-service/total?logo=github&color=green)](https://github.com/LussacZheng/dandanplay-resource-service/releases)
[![jsDelivr hits](https://img.shields.io/jsdelivr/gh/hm/LussacZheng/dandanplay-resource-service?color=red)](https://data.jsdelivr.com/v1/package/gh/LussacZheng/dandanplay-resource-service@dist/stats/file)

API implementations for "dandanplay" resource search service.  
[弹弹play](http://www.dandanplay.com/) 资源搜索节点的 API 实现。

提供分别基于 [**Cloudflare Workers**](#cloudflare-workers) 和 [**Golang**](#golang) 的两种实现。

## 特性

- Serverless 优先
- 支持 [搜索指令](docs) : `$realtime` , `$page` , `$limit`

---

## Cloudflare Workers

注册一个 Cloudflare 免费账户即可部署，**无需 远程服务器 或 常驻任何本地程序** 。

### 部署

复制 [`worker.js`](https://github.com/LussacZheng/dandanplay-resource-service/blob/dist/cf-worker/worker.js) 内容到 [workers.dev 脚本编辑页面](https://workers.cloudflare.com/) 中，部署即可。

> - 可在搜索引擎中搜索 "`Cloudflare Workers部署教程`" ，参照进行。
> - 由于暂未实现 [缓存机制](https://developers.cloudflare.com/workers/runtime-apis/cache) ，且 Cloudflare Workers 免费版账户有每日 100,000 的请求次数限制，目前暂不提供演示站点。可以自行部署体验。

### 开发

若需修改源码，则需要 [Node.js](https://nodejs.org/) 开发环境。可以依照以下步骤自行生成 `worker.js` :

1. 安装 [wrangler](https://github.com/cloudflare/wrangler) 。
   > 若为 Windows 系统，建议选择 [二进制文件安装](https://developers.cloudflare.com/workers/cli-wrangler/install-update#manual-install) 。

2. 修改源码并重新构建：

   ```shell
   $ git clone https://github.com/LussacZheng/dandanplay-resource-service.git
   $ cd dandanplay-resource-service/cf-worker
   $ npm install

   # 按需修改源码

   $ wrangler build
   # or
   $ npm run dist
   ```

3. 输出文件位于 `cf-worker/dist/worker.js` 。

---

## Golang

### 部署

从 [Releases 页面](https://github.com/LussacZheng/dandanplay-resource-service/releases) 下载预编译的可执行文件到 本地 或 服务器 ，(重命名后)运行即可。

若不想部署到本地或服务器，可直接部署到 Heroku :  
[![Deploy to Heroku](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/LussacZheng/dandanplay-resource-service)

### 运行

```shell
$ dandanplay-resource-service -h
API implementations for "dandanplay" resource search service, in Golang.

Usage:
  dandanplay-resource-service [flags]

Flags:
      --debug          以调试模式运行，运行时输出更多信息
      --dry-run        阻止网络访问并返回空数据，用于开发调试
  -h, --help           输出帮助信息
  -H, --host string    API 监听的 IP 地址，例如 "0.0.0.0", "127.0.0.1", "192.168.0.100" (默认为 "localhost")
  -P, --port string    API 监听的端口 (默认为 "8080")
  -x, --proxy string   网页解析器所使用的代理地址，支持 "http" 和 "socks5" 协议
  -V, --version        输出版本信息
```

例如：

```shell
# 无参数，默认运行在 localhost:8080
$ dandanplay-resource-service

# 自定义端口 和 代理地址(可省略 "http://")
$ dandanplay-resource-service -P 9000 -x 127.0.0.1:10809

# 运行在 34543 端口并暴露服务到公网
$ dandanplay-resource-service -H 0.0.0.0 -P 34543
```

访问根路径可以看到简易的测试页面。

### 开发

若需修改源码或自行编译，则需要 [Go 语言](https://golang.google.cn/) 开发环境。

```shell
$ git clone https://github.com/LussacZheng/dandanplay-resource-service.git
$ cd dandanplay-resource-service/golang

# go env -w GO111MODULE=on
# go env -w GOPROXY=https://goproxy.cn,direct

# 直接编译
$ go build

# 优化可执行文件大小
$ go build -ldflags="-s -w"
$ upx --lzma --best dandanplay-resource-service*
```

---

## 相关项目

### Python

- [源代码](https://pastebin.ubuntu.com/p/mGP7JRpBtd/) / [参考](https://support.qq.com/products/104929/post/161035286010443896/)

<details>
<summary>个人推荐使用 <a href="https://github.com/python-poetry/poetry">poetry</a> 进行依赖管理。</summary>

```shell
$ cd python

# 安装依赖
$ poetry install
# 或
$ poetry install --no-dev

# 运行脚本，如
$ poetry run python dandanplay-resource-service.py proxy=http://127.0.0.1:10809
# 或
$ poetry run python dandanplay-resource-service.py host=0.0.0.0 port=34543 proxy=""

# 打包
$ poetry run poe build
```

</details>

### PHP

- [lianxun/dandan - Gitee](https://gitee.com/lianxun/dandan)

## 参考资料

1. **API Specification** - [弹弹play资源搜索节点API规范](https://github.com/kaedei/dandanplay-libraryindex/blob/master/api/ResourceService.md)
2. [关于“资源搜索”功能即将下线的通知](https://mp.weixin.qq.com/s/0xzIJX2LWnncc2YKpe6sfw)
3. [资源搜索相关问题的解答](https://mp.weixin.qq.com/s/OSsk6tuj4lXMcKq2S4s1Kg)

---

## TODO

- [ ] 添加 Dockerfile
- [ ] 引入缓存机制
- [ ] _[go-impl]_ 完善 KissSub 相关网页的 [解析](https://github.com/LussacZheng/dandanplay-resource-service/tree/main/golang/api/kisssub#known-issues)
- [ ] _[go-impl]_ 添加更多的信息源/发布站

## Contributing

Issues, pull requests, and discussions are always welcome.

## License

This project is licensed under the [Apache-2.0 License](./LICENSE).
