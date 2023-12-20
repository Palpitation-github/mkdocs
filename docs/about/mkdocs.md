---

tags:
  - Website

---

# 站点主题-Material for MkDocs

## 安装-Installation

### 网页端

登录 Github 并新建名为`mkdocs`的仓库，设置为 Public（公开仓库），添加 README 文件，添加`.gitignore`文件并设置为 Python 模板，选择 MIT 许可证。复制 git clone 地址，如 ssh 方式地址：`git@github.com:Palpitation-github/mkdocs.git`。【SSH配置另说】

### 本地端

Windows 下选择一个不包含中文路径的文件夹，右键使用 TortoiseGit 软件克隆上面的仓库（或打开终端使用对应的 Git 命令）。

#### 配置环境

在此文件夹右键打开终端，输入`❯ code .`打开 Visual Studio Code，修改`.gitignore`文件将`poetry.lock`这一行注释取消。

打开 Visual Studio Code 终端，输入`❯ poetry init`初始化 Poetry 项目。根据终端提示设置项目名称（注意不要与 Python 包相同）、作者、描述、Python 版本、许可证等信息。配置选项`Would you like to define your main dependencies interactively? (yes/no)`和`Would you like to define your development dependencies interactively? (yes/no)`都选择`no`，`Do you confirm generation? (yes/no) [yes]`选择`yes`或直接回车生成项目，Poetry 会自动生成项目的配置文件`pyproject.toml`。

终端输入`❯ poetry source add --priority=default mirrors https://pypi.tuna.tsinghua.edu.cn/simple/`设置为[清华镜像源](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)，项目配置文件大致如下：

``` toml title="pyproject.toml" hl_lines="13-16"

[tool.poetry]
name = "demo"
version = "0.1.0"
description = ""
authors = [""]
license = "MIT"
readme = "README.md"

[tool.poetry.dependencies]
python = "^3.10"


[[tool.poetry.source]]
name = "mirrors"
url = "https://pypi.tuna.tsinghua.edu.cn/simple/"
priority = "default"

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"

```

终端中输入`❯ poetry add mkdocs-material`，Poetry 会自动创建 Python 虚拟环境（若之前配置过环境变量则在当前目录生成`.venv`文件夹）并安装 mkdocs-material 包（包含 mkdocs 和其他包）。

终端输入`❯ mkdocs new .`，创建 mkdocs 项目（重启 Visual Studio Code 会自动激活虚拟环境而不必使用 `poetry run mkdocs new .`命令）。mkdocs 项目目录大致如下

``` bash title="项目目录"
mkdocs
├─ .venv
├─ docs
│	└─ index.md
├─ mkdocs.yml
├─ poetry.lock
└─ pyproject.toml

```
`/docs`文件夹主要用于保存页面的`.md`文档，`mkdocs.yml`为 mkdocs-material 的配置文件。在终端使用`❯ mkdocs serve`命令启动 mkdocs 服务，在浏览器中访问提示的地址就可看到生成的页面。编辑`mkdocs.yml`修改主题为`material`后保存，mkdocs-material 会自动更新配置并刷新页面。

``` yaml title="mkdocs.yml" hl_lines="3-5"
# 站点名称
site_name: My Docs
theme:
  # 主题名称
  name: material

```

<figure markdown>
  ![Material for Mkdocs](https://squidfunk.github.io/mkdocs-material/assets/screenshots/creating-your-site.png){ loading=lazy height="100px" width="400px"}
  <figcaption>Material for Mkdocs 截图来源：Github</figcaption>
</figure>

## 设置-Setup


支持的图标可在此处查

[:fontawesome-solid-font-awesome:{ .fa-thumbs-up } FontAwesome ](https://fontawesome.com/search?m=free)
[:material-material-design:{ .fa-thumbs-down } Material Design ](https://pictogrammers.com/library/mdi/)
[:octicons-mark-github-16:{ .fa-thumbs-up } Octicons ](https://primer.style/foundations/icons)
[:simple-simpleicons:{ .blink } Simple Icons ](https://simpleicons.org/)

图标动效天气

完整的配置文件可参考我的[:fontawesome-solid-file:{ .heart } mkdocs.yml ](https://github.com/Palpitation-github/mkdocs/blob/main/mkdocs.yml)

## 个性化-Customization

## 发布-Publish

### 本地端

新建`.github/workflows/ci.yml`文件，内容如下:

``` yaml title="ci.yml"
name: ci
on:
  push:
    branches:
      - master
      - main
permissions:
  contents: write
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Configure Git Credentials
        run: |
          git config user.name github-actions[bot]
          git config user.email 41898282+github-actions[bot]@users.noreply.github.com
      - uses: actions/setup-python@v4
        with:
          python-version: 3.x
      - run: echo "cache_id=$(date --utc '+%V')" >> $GITHUB_ENV
      - uses: actions/cache@v3
        with:
          key: mkdocs-material-${{ env.cache_id }}
          path: .cache
          restore-keys: |
            mkdocs-material-
      - run: pip install mkdocs-material
      - run: mkdocs gh-deploy --force

```
推送

### 网页端

推送完成后打开仓库 Settings 设置，选择 Pages 页面，将 Branch 分支设置为 `gh-pages`，然后点击 Save 保存，Github Actions 就会自动部署并在 xxx.github.io/mkdocs/生成网页，如我的为`https://palpitation-github.github.io/mkdocs/`。