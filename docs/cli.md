# 命令行工具

本项目提供了名为`unipie`的命令行工具，用于在终端中处理Unifont相关任务。

安装`unifont_utils`后，`unipie`会自动加入系统路径以供调用。

## 帮助

- 全局帮助：`unipie --help`
- 查看子命令帮助（示例）：`unipie edit file --help`

## 交互式编辑

### `edit file`

在现有`.hex`文件中编辑指定码位。

- 主要参数：
  - `-p, --font_path/--path`：输入的`.hex`文件路径，必填。
  - `-c, --code_point/--cp`：目标码位（如`4E00`），必填。
  - `-o, --output`：输出文件路径，若提供则忽略`--overwrite`。默认为`<原文件名>_edited.hex`。
  - `--overwrite/--no-overwrite`：未指定`--output`时是否覆盖原文件，默认为`--no-overwrite`。

示例：

```shell
unipie edit file -p ./unifont.hex -c 4E00 --overwrite
```

### `edit str`

编辑单个`.hex`字形字符串。

- 主要参数：
  - `-c, --code_point/--cp`：目标码位，必填。
  - `-s, --str/--hex_str/--hex`：待编辑的`.hex`字符串，必填。

示例：

```shell
unipie edit str -c 4E01 -s 00007FFC01000100010001000100010001000100010001000100010005000200
```

### `edit empty`

为指定码位创建空白字形并进入编辑，默认宽度16。

- 主要参数：
  - `-c, --code_point/--cp`：目标码位，必填。
  - `-w, --width`：字形宽度，默认为16。

示例：

```shell
unipie edit empty -c 4E02
```

## 操作`.hex`文件

### `hex add`

向`.hex`文件新增码位。

- 主要参数：
  - `-p, --font_path/--path`：输入的`.hex`文件路径，必填。
  - `-c, --code_point/--cp`：目标码位，必填。
  - `-s, --hex_str/--hex`：要写入的`.hex`字符串，必填。
  - `-o, --output`：输出文件路径，若提供则忽略`--overwrite`。默认为`<原文件名>_edited.hex`。
  - `--overwrite/--no-overwrite`：未指定`--output`时是否覆盖原文件，默认为`--no-overwrite`。

示例：

```shell
unipie hex add -p ./unifont.hex -c 4E01 -s 00007FFC01000100010001000100010001000100010001000100010005000200
```

### `hex replace`

替换已有码位的字形数据。

- 主要参数：
  - `-p, --font_path/--path`：输入的`.hex`文件路径，必填。
  - `-c, --code_point/--cp`：目标码位，必填。
  - `-s, --hex_str/--hex`：新的`.hex`字符串，必填。
  - `-o, --output`：输出文件路径，若提供则忽略`--overwrite`。默认为`<原文件名>_edited.hex`。
  - `--overwrite/--no-overwrite`：未指定`--output`时是否覆盖原文件，默认为`--no-overwrite`。

示例：

```shell
unipie hex replace -p ./unifont.hex -c 4E01 -s 00007FFC01000100010001000100010001000100010001000100010005000200
```

### `hex delete`

删除指定码位。

- 主要参数：
  - `-p, --font_path/--path`：输入的`.hex`文件路径，必填。
  - `-c, --code_point/--cp`：目标码位，必填。
  - `-o, --output`：输出文件路径，若提供则忽略`--overwrite`。默认为`<原文件名>_edited.hex`。
  - `--overwrite/--no-overwrite`：未指定`--output`时是否覆盖原文件，默认为`--no-overwrite`。

示例：

```shell
unipie hex delete -p ./unifont.hex -c 4E01 --overwrite
```

### `hex view`

在终端渲染指定码位字形。

- 主要参数：
  - `-p, --font_path/--path`：输入`.hex`文件路径，必填。
  - `-c, --code_point/--cp`：目标码位，必填。

示例：

```shell
unipie hex view -p ./unifont.hex -c 4E01
```

### `hex query`

仅输出指定码位的`.hex`字符串，可静默模式。

- 主要参数：
  - `-p, --font_path/--path`：输入`.hex`文件路径，必填。
  - `-c, --code_point/--cp`：目标码位，必填。
  - `--pure/--verbose`：是否静默输出，仅打印结果字符串，默认`--verbose`。

示例：

```shell
unipie hex query -p ./unifont.hex -c 4E01 --pure
```

## 转换

### `convert hex2img`

将`.hex`字符串导出为图片，支持指定颜色方案与图片格式。

- 主要参数：
  - `-s, --str/--hex_str/--hex`：源`.hex`字符串，必填。
  - `-o, --output`：输出图片路径，必填。
  - `-f, --img_format/--format`：输出图片格式，默认`PNG`。
  - `-c, --color_scheme`：颜色方案，默认为`black_and_white`。

示例：

```shell
unipie convert hex2img -s 00007FFC01000100010001000100010001000100010001000100010005000200 -o ./glyph.png -f PNG -c black_and_white
```

### `convert img2hex`

从图片生成`.hex`字符串，支持自动检测或手动指定颜色方案。

- 主要参数：
  - `-p, --img_path/--path`：输入图片路径，必填。
  - `-a, --auto_detect/--auto`：是否自动检测颜色方案，默认不启用。
  - `-c, --color_scheme`：手动指定颜色方案，为空则自动检测颜色方案。

示例：

```shell
unipie convert img2hex -p ./glyph.png -a
```

## 下载

### `download`

下载并解压官方发行的GNU Unifont `.hex`文件。

- 主要参数：
  - `-v, --version`：Unifont版本（>=7.x），留空则下载最新。
  - `-t, --variant`：发行变体，默认为`unifont_all`，可选项：
    - `unifont`
    - `unifont_all`
    - `unifont_jp`
    - `unifont_jp_sample`
    - `unifont_sample`
    - `unifont_upper`
    - `unifont_upper_sample`
  - `-o, --output`：输出`.hex`路径，未指定则使用`<variant>-<version>.hex`。
  - `-f, --force`：输出时覆盖已存在的同名文件。
  - `--timeout`：下载超时时间（秒），默认为30。

示例：

```shell
unipie download -v 16.0.04 -t unifont_jp -f
```

### info

显示 Unipie 版本信息。
