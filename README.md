# Sub2API 插件模板

使用此模板创建插件仓库。仓库自带源码下载、测试、编译和安装器打包流程。
可以用它构建单插件版本，也可以参考 [Sub2API-Plugins](https://github.com/Nanboom233/Sub2API-Plugins) 构建多插件组合版本。

## Getting Started

在 GitHub 点击 **Use this template** 创建仓库，然后构建：

```sh
python3 scripts/build.py
```

未设置 `UPSTREAM_TAG` 时，脚本使用上游最新正式 Release 的 tag。如需构建指定版本，设置 `UPSTREAM_TAG`：

```sh
UPSTREAM_TAG=v0.2.19 python3 scripts/build.py
```

该命令自动下载上游源码、应用补丁、运行测试，并生成：

```text
dist/build/<os>-<arch>/sub2api-plugins-<upstream_tag>-<os>-<arch>
dist/build/<os>-<arch>/sub2api-plugins-installer-<upstream_tag>-<os>-<arch>
```

构建覆盖 `linux-amd64`、`linux-arm64`、`darwin-amd64`、`darwin-arm64` 和 `windows-amd64`。模板没有业务补丁，直接构建会得到未修改的上游 Sub2API 和对应安装器。

## Patches

把标准 Git diff 放在 `patches/*.patch`。补丁路径相对于 Sub2API 源码根目录。二进制变更使用 `git diff --binary`。

构建按文件名顺序应用补丁。可以用数字前缀控制顺序，不需要 `series` 文件。多个插件修改同一个文件时，后面的补丁必须能应用到前面补丁修改后的源码上。

任一补丁应用失败，构建都会立即失败。

## Tests

测试代码和支持文件平铺在 `tests/`，测试数据放在 `assets/`。构建默认发现并运行：

- `test_*.py`
- `test_*.go`
- `test_*.cjs`
- `test_*.test.ts`

原生 Go 测试使用 `test_*_test.go`，并声明目标包，例如：

```go
// sub2api-package: backend/internal/service
package service
```

构建会把测试临时放入对应源码包。Vue 测试使用下载源码中的 Vitest 环境。测试失败会使构建失败。

## CI and Installation

推送、pull request 和手动运行都会触发 [`.github/workflows/build.yml`](.github/workflows/build.yml)。

成功的运行直接提供各平台的服务器程序和安装器，用法见 [Sub2API-Plugins](https://github.com/Nanboom233/Sub2API-Plugins#installation)。

## License

插件采用 `LGPL-3.0-or-later`。根目录的 `LICENSE` 和 `licenses/` 保存适用的许可证和原始署名。

## Credits

- [Sub2API](https://github.com/Wei-Shaw/sub2api)：本项目使用的管理平台。
- [LINUX DO](https://linux.do/)：新的理想型社区。
