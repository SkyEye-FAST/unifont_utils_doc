# 下载器

`UnifontDownloader`用于下载并解压官方发布的GNU Unifont`.hex`文件

## 快速开始

``` python
from unifont_utils import UnifontDownloader

# 下载最新的unifont_all变体并写入当前目录
path, version = UnifontDownloader().download_hex()
print(f"Downloaded {version} to {path}")
```

指定版本、变体和输出路径：

``` python
downloader = UnifontDownloader(timeout=60)
path, version = downloader.download_hex(
    version="16.0.04",
    variant="unifont_jp",
    output="cache/unifont_jp-16.0.04.hex",
    force=True,  # 已存在文件时覆盖
)
```

- 版本号格式为`<major>.<minor>.<patch>`（如`17.0.03`），`<major>`最低为7。
- 未指定`output`时，默认写入`<variant>-<version>.hex`。

## 版本变体与版本号校验

`normalize_variant`会校验并规范化传入的版本变体，可选项有：

- `unifont`
- `unifont_all`
- `unifont_jp`
- `unifont_jp_sample`
- `unifont_sample`
- `unifont_upper`
- `unifont_upper_sample`

`normalize_version`支持带前缀的写法（如`v17.0.03`）。

## 进度回调

下载过程中可以通过`progress_callback`获取实时进度：

``` python
def show_progress(downloaded: int, total: int | None) -> None:
    if total:
        percent = downloaded * 100 / total
        print(f"\r{percent:5.1f}% ({downloaded}/{total} bytes)", end="")
    else:
        print(f"\r{downloaded} bytes", end="")

UnifontDownloader().download_hex(progress_callback=show_progress)
print()  # 换行
```

当服务器未返回`Content-Length`时，`total`为`None`，此时只能显示已下载的字节数。

## CLI对应命令

命令行等价实现为`unipie download`，参见[命令行工具说明](cli.md)：

``` shell
unipie download -v 16.0.04 -t unifont_jp -o unifont_jp-16.0.04.hex --force
```
