

# 使用Sphinx+Git+ReadtheDocs搭建个人博客

## 背景

为了快速的搭建一个个人的文档平台，并且避免使用复杂的前后端工具，最后选择使用Sphinx+Git+ReadtheDocs来搭建我的个人博客，其中经历了很多复杂的试探，这里不得不吐槽一下Sphinx的官方文档看起来真是痛苦。每个工具的用途如下：

* **Sphinx**：将文档生成为HTML的文件，因为习惯了使用MarkDown语法，所以将Sphinx配置为解析md文件。
  * MD编写的文件推荐使用Typora，可见即所得。
* **Git**：用于托管代码，便于进行版本管理。
* **ReadtheDocs**：与Git进行关联之后可以在Git提交之后，自动的构建HTML文件。

## 建立Git仓库

* 登录地址：[GitHub](https://github.com/)，然后创建文档的仓库`VanLuo-Docs`
* 克隆至本地

## 安装Sphinx

* 使用pip命令进行安装

```py
pip install -U sphinx
```

* 使用命令切换至Git创建的目录，然后执行快速创建项目的命令

```bash
sphinx-quickstart
```

* 当提示`Separate source and build directories (y/n) [n]:`时，选择y，这样会将源文件与构建的文件分开：
  * source：存储文档的源文件
  * build：基于source构建的Html文件
* 其它的就是设置项目名、用户名即可
* 在git中将build文件排查，不用上传，使用ReadtheDocs自动构建即可

### 配置Sphinx

打开`config.py`文件进行配置

#### 配置主题

默认的主题非常丑，先登陆[https://sphinx-themes.org/](https://sphinx-themes.org/)，选择一个你喜欢的主题，在这里我以`sphinx_rtd_theme`为例。

* 使用pip安装主题

```python
pip install sphinx_rtd_theme
```

* 在配置文件中添加如下配置项

```python
# on_rtd is whether we are on readthedocs.org
import os
on_rtd = os.environ.get('READTHEDOCS', None) == 'True'

if not on_rtd:  # only import and set the theme if we're building docs locally
    import sphinx_rtd_theme
    html_theme = 'sphinx_rtd_theme'
    html_theme_path = [sphinx_rtd_theme.get_html_theme_path()]
```

#### 配置MarkDown语法

学习MarkDown已经很费劲了，更不要说再学习Sphinx的reStructuredText 语法了，使用如下的方法可以读取md文件，但是我还没有找到生成目录的好方法，目录继续使用.rst文件，配置的[参考文档](https://www.sphinx.org.cn/usage/markdown.html)。

* 使用pip安装转换器

```python
pip install recommonmark
```

* 修改配置文件，设置编译器，以及sphinx支持的文件格式

```python
from recommonmark.parser import CommonMarkParser
source_parsers = {
    '.md': CommonMarkParser,
}
source_suffix = ['.rst', '.md']
```

* 千万不要配置`extensions = ['recommonmark']`，不然会引起报错

## 写Sphinx文档

### 写目录

`toctree`可以把多个文档合并为一个，`maxdepth`是设置目录层级的参数，包含内容中间间隔一行，用/来区分文件夹，toctree的参数可以查看[链接](https://www.sphinx.org.cn/usage/restructuredtext/directives.html#toctree-directive)。

```reStructuredText
工具
================================
.. toctree::
   :maxdepth: 1
   :glob:
   :titlesonly:

   *
```

* 如果不想使用自带的文档标题，可以使用如下的语法：

```reStructuredText
All about strings <strings>
```

* 参数：
  
  * :numbered: 添加节号
  * :glob:使用模糊匹配
  * :titlesonly: 只显示文档标题
  * :includehidden: 只显示顶级toctree

###  侧边栏导航

* 需要新建一个contents.rst文件，用来生成侧边栏目录

### 上传编写的文档

* 使用git上传你编写好的文档。

## 使用readthedocs构建页面

* 打开[readthedocs网址](https://readthedocs.org/)，注册账号，并且import Git的项目

<img src="../_static/使用Sphinx+Git+ReadtheDocs搭建个人博客.assets/image-20200507142956507.png" alt="image-20200507142956507" style="zoom:50%;" />

* 进入项目进行设置，切换语言为简体中文。
* 至此，每次的git提交都会触发页面的构建工作。

## 参考文档

* [Google 开源项目风格指南 (中文版)](https://github.com/zh-google-styleguide/zh-google-styleguide)：作为编写文档格式的参考
* [搭建教程](https://www.xncoding.com/2017/01/22/fullstack/readthedoc.html)
* [Spinx官方中文文档](https://www.sphinx.org.cn/index.html)

Typora

* [用Typora画图](http://support.typora.io/Draw-Diagrams-With-Markdown/)
* [mermaid](https://mermaid-js.github.io/mermaid/#/)

