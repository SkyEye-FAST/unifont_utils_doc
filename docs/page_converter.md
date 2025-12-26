# 分页转换

分页转换用于在分页（256个码位）的`.hex`数据与图片之间互相转换，布局兼容`unihex2bmp`：尺寸固定为576×544，包含页码抬头和16×16的字符网格。

## 从`.hex`文件渲染分页图片

``` python
from unifont_utils import GlyphSet
from unifont_utils.page_converter import save_page_image

glyphs = GlyphSet.load_hex_file("unifont.hex")
save_page_image(
    glyphs,
    page="83",  # 十六进制页号
    output="u83.png",
    color_scheme="inverted_black_and_white",
)
```

- `hex_page_to_image`返回`PIL.Image`对象，适合进一步处理；`save_page_image`直接落盘。
- 输出格式可通过`img_format`指定；若省略，则由文件扩展名推断（默认为PNG）。
- 配色方案的映射规则见[相关说明](color_scheme.md)。

## 从分页图片生成`.hex`文件

``` python
from unifont_utils.page_converter import image_to_hex_page

glyphs = image_to_hex_page(
    "u83.png",
    page="83",
    color_auto_detect=True,  # 默认启用自动检测
    skip_blank=True,         # 跳过全空白字形以减少输出
)

glyphs.save_hex_file("u83.hex")
```

- 图片尺寸必须是576×544；否则会抛出`ValueError`。
- 关闭自动检测时，需要通过`color_scheme`显式指定配色方案：

``` python
image_to_hex_page("u83.png", page="83", color_auto_detect=False, color_scheme="black_and_white")
```

- 当`skip_blank=False`时，空白字形也会写回输出。

## CLI对应命令

命令行等价实现为`unipie convert page`，参见[命令行工具说明](cli.md)：

- 渲染分页图片：

``` shell
unipie convert page hex2img -p unifont.hex -g 83 -o u83.png -c black_and_white
```

- 分页图片转`.hex`：

``` shell
unipie convert page img2hex -p u83.png -g 83 -o u83.hex --auto_detect
```
