---
layout: post
title:  "import-pypi 学习, 如何优雅的在ipython中安装插件"
date:   2018-04-21 22:44:06 +0800
comments: true
tags:
- programming
- python
---

在用`ipython`的时候总会遇到package并不能import的问题，一般情况可能想到的是用退出安装，或者是用`!pip install package`来安装，现在通过另外一种方式(`import-pypi`)来下载需要加载的包

## 方式1

``` bash
In [1]: import packagename
---------------------------------------------------------------------------
ModuleNotFoundError                       Traceback (most recent call last)
<ipython-input-1-6d7d6f569116> in <module>()
----> 1 import packagename

ModuleNotFoundError: No module named 'packagename'
# 此时将pypi将自动搜索包名，然后进行安装
In [2]: import pypi
```

## 方式二

``` python
import pypi
pypi.install('Django', '2.0.3')

import django

print(django.__version__)  # 2.0.3
```

## 学习准备

### `import module`的时候`python`做了哪些工作

#### python导入协议

##### 导入过程

Python中所有加载到内存的模块都放在`sys.modules`。当import一个模块时首先会在这个列表中查找是否已经加载了此模块,如果加载了则只是将模块的名字加入到正在调用 import 的模块的 `Local` 名字空间中。如果没有加载则从sys.path目录中按照模块名称查找模块文件,模块文件可以是 py、pyc、pyd,找到后将模块载入内存,并加入到 sys.modules 中,一般如果 指定模块没在 sys.modules 中找到,将调用Python 的导入协议来查找和加载 模块。 

##### 导入过程中使用的工具

python 导入协议机制是由查找器finder和加载器loader构成. 可以和git hook比较

###### finder

查找器finder的职责是找到指定的模块,然后提供一个加载器来处理实际的导入 行为。要注册一个查找器只需要在 sys.path_hooks 列表中。查找器不真正加载 模块。如果他们能够找到指定的模块,他们返回一个模块分支,即模块导入相 关信息的封装,在加载模块时导入机制运用的信息。 
查找器对象finder需要实现如下find_module 方法： 
finder.find_module(fullname, path=None)1 

* 如果 finder 被装入了 meta_path,这个方法会接受第二个参数, 
* 如果 Import 的是顶层模块:None, 
* 如果 import 的是子模块或子包:PATH 
* 如果找到模块需要返回一个 loader 对象,没找到需要返回 None 

###### loader

加载器loader需要实现如下load_module 方法 
loader.load_module(fullname): 

* 这个函数返回一个被载入的模块或者抛出异常 
* 判断传入的 fullname 是不是已经在 sys.modules 中了,如果在的话loader 必须使用已经载入的模块,否则 reload 功能不能正常工作,如果fullname 不在 sys.modules 中,loader 必须新建一个 module 并载入 
* 在 Loader 执行导入的模块代码之前,模块必须已经被导入 sys.modules中,否则会引起无穷迭代 
* 如果导入失败,loader 必须清除已经插入到 sys.modules 中的模块 

通过自己实现这样的包括查找器和加载器钩子程序能够实现对 python 导入行为的控制，扩展Python加载模块的功能。

#### `sys.meta_path` Python import hook可以翻译为Python 探针

python 模块是通过import的方式引用的，当我们执行一行 from package import module as mymodule 命令时，Python解释器会查找package这个包的module模块，并将该模块作为mymodule引入到当前的工作空间。import语句主要是做了二件事： 查找相应的module 和加载module到local namespace。

在import的第一个阶段，主要是完成了查找要引入模块的功能，这个查找的过程如下：

1. 检查 sys.modules列表 (保存了之前import的类库的缓存），如果module被找到，则⾛到第二步。
2. 检查 sys.meta_path列表。meta_path 是一个 list，⾥面保存着一些 finder 对象，如果找到该module的话，就会返回一个finder对象。
3. 检查某些隐式的finder对象，不同的python实现有不同的隐式finder，但是都会有 sys.path_hooks, sys.path_importer_cache 以及sys.path。 
4. 抛出 ImportError 

下面是一个简单的hellp world程序 我们通过import hook实现在加载模块时 打印查找和加载的信息：

``` python
import sys

class MetaPathFinder:

    def find_module(self, fullname, path=None):
        print('find_module {}'.format(fullname))
        return MetaPathLoader()


class MetaPathLoader:

    def load_module(self, fullname):
        print('load_module {}'.format(fullname))
        sys.modules[fullname] = sys
        return sys

sys.meta_path.insert(0, MetaPathFinder())

if __name__ == '__main__':
    import http
    print(http)
    print(http.version_info)
```

运行结果

``` bash
$ python meta_path1.py
find_module http
load_module http
<module 'sys' (built-in)>
sys.version_info(major=3, minor=5, micro=1, releaselevel='final', serial=0)
```


### importlib库

#### python2和python3中的不同

python2中importlib只是提供了一个从字符串导入包的函数

``` python
import importlib
print(importlib.import_module("sys"))
print(importlib.import_module("sys").version)
```

python3中importlib提供了更多的方法

#### python3中的importlib
importlib模块的目的

1. 提供了`import`语句和`__import__`函数的实现
2. 允许程序员创建自定义对象，这个对象可以用于导入过程(import process)中

这个模块很复杂，所以主要学习三个部分：

* 动态导入(dynamic import)
* 检测模块是否可以被导入(checking is a module can be imported)
* 从源文件中导入(importing from source file itself)

#### dynamic import

创建一个如下的目录
``` bash
.
├── bar.py
├── foo.py
└── importer.py
```

文件中代码如下

``` python
# bar.py foo.py
def main():
    print(__name__)
```

``` python
# importer.py
import importlib

def dynamic_import(module):
    return importlib.import_module(module)
    
if __name__ == '__main__':
    module = dynamic_import('foo')
    module.main()
    module_two = dynamic_import('bar')
    module_two.main()
```

``` bash
$ python3 importer.py
```
通过上面代码知道，只要知道模块的字符串表示，就能通过`importlib.import_module`函数来加载模块，并且函数将返回加载的模块对象。这个方法提供了一种动态导入模块的方法。

#### module import checking

``` python
 import importlib

 def check_module(module_name):
     """ 检测模块是否可以被导入，不用真正的导入模块
     """
     module_spec = importlib.util.find_spec(module_name)
     if module_spec is None:
         print("Module: {} not found.".format(module_name))
         return None
     else:
         print("Module: {} can be imported.".format(module_spec))
         return module_spec

 def import_module_from_spec(module_spec):
     """ 通过module specification导入模块
     返回模块对象
     """
     module = importlib.util.module_from_spec(module_spec)
     module_spec.loader.exec_module(module)
     return module

 if __name__ == '__main__':
     module_spec = check_module('fake_module')
     module_spec = check_module('collections')
     if module_spec:
         module = import_module_from_spec(module_spec)
         print(dir(module))
```

#### Import From Source File

``` python
import importlib.util
 
def import_source(module_name):
    module_file_path = module_name.__file__
    module_name = module_name.__name__
 
    module_spec = importlib.util.spec_from_file_location(
        module_name, module_file_path)
    module = importlib.util.module_from_spec(module_spec)
    module_spec.loader.exec_module(module)
    print(dir(module))
 
    msg = 'The {module_name} module has the following methods:' \
        ' {methods}'
    print(msg.format(module_name=module_name, 
                     methods=dir(module)))
 
if __name__ == '__main__':
    import logging
    import_source(logging)
```



## import-pypi源码分析

``` python
import importlib.abc
import importlib.machinery
import subprocess
import sys


def install(pkgname, version=None):
    cmd = [sys.executable, '-m', 'pip', 'install']
    if version:
        cmd.append('{}=={}'.format(pkgname, version))
    else:
        cmd.append(pkgname)
    subprocess.check_call(
        cmd,
        stdout=subprocess.DEVNULL,
        stderr=subprocess.DEVNULL,
    )


class PipFinder(importlib.abc.MetaPathFinder):

    # https://github.com/python/cpython/blob/master/Lib/importlib/abc.py#L50-L81 
    # 看链接里的代码就能知道为什么要这样写了
    def find_spec(self, fullname, path, target=None):
        try:
            install(fullname)
        except subprocess.CalledProcessError:
            return None
        else:
            return importlib.machinery.PathFinder().find_spec(
                fullname,
                path,
                target,
            )


# 注册
sys.meta_path.append(PipFinder())
```

`import-pypi`其实就是在python导入模块的过程中，对于寻找模块失败的时候，做了一个自动下载的功能，并且只要导入`import-pypi`模块的时候，将在sys.meta_path中注册一个能实现自动下载未寻找到的模块的finder，这样每次导入模块的时候都能实现这个功能。

引用:
1. https://blog.csdn.net/u010786109/article/details/52038443
2. https://www.blog.pythonlibrary.org/2016/05/27/python-201-an-intro-to-importlib/
3. https://docs.python.org/3.5/library/importlib.html
