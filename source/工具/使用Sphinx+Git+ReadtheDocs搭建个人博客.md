

# 使用Sphinx+Git+ReadtheDocs搭建个人博客

## 背景

为了快速的搭建一个个人的文档平台，并且避免使用复杂的前后端工具，最后选择使用Sphinx+Git+ReadtheDocs来搭建我的个人博客，其中经历了很多复杂的试探，这里不得不吐槽一下Sphinx的官方文档看起来真是痛苦。每个工具的用途如下：

* **Sphinx**：将文档生成为HTML的文件，因为习惯了使用MarkDown语法，所以将Sphinx配置为解析md文件。
  * MD编写的文件推荐使用Typora，可见即所得。
* **Git**：用于托管代码，便于进行版本管理。
* **ReadtheDocs**：与Git进行关联之后可以在Git提交之后，自动的构建HTML文件。

## 建立Git仓库

* 登录地址：https://github.com/，然后创建文档的仓库`VanLuo-Docs`
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

默认的主题非常丑，先登陆https://sphinx-themes.org/，选择一个你喜欢的主题，在这里我以`sphinx_rtd_theme`为例。

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

学习MarkDown已经很费劲了，更不要说再学习Sphinx的reStructuredText 语法了，使用如下的方法可以读取md文件，但是我还没有找到生成目录的好方法，目录继续使用.rst文件。

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



## 参考文档

* [Google 开源项目风格指南 (中文版)](https://github.com/zh-google-styleguide/zh-google-styleguide)：作为编写文档格式的参考

