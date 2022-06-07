# gitBook篇

## 1 word文档转markdown
writage 插件，下载地址http://www.writage.com/#download

下载安装完成后，重新打开word，保存原word文档为markdown类型即可

## 2 gitbook客户端

前提：得安装nodejs，我选的官网的[绿色版](https://nodejs.org/zh-cn/download/)

安装客户端：`npm install gitbook-cli –g`

gitbook v2.3.2有一个bug：

```
D:\work\node-v16.15.1-win-x64\node_modules\gitbook-cli\node_modules\npm\node_modules\graceful-fs\polyfills.js
```

这个文件的statFix函数调用需要被注释，否则会报`cb.apply is not a function`：

```
//fs.stat = statFix(fs.stat)
//fs.fstat = statFix(fs.fstat)
//fs.lstat = statFix(fs.lstat)
```

## 3 使用gitbook

### 3.1 初始化

进入要存储的位置初始化gitbook: gitbook init

出现两个文件，README.md是说明文档，SUMMARY.md是书的章节目录

SUMMARY.md的初始化内容：

```
# Summary
* [Introduction](README.md)
```

- 指定端口：`gitbook serve --port 2333`

- 生成pdf格式：`gitbook pdf ./ ./mybook.pdf`

### 3.2 转成网页
gitbook serve：该命令后会在书籍的文件夹中生成一个 \_book 文件夹, 里面的内容即为生成的 html文件，并开启服务器（http://localhost:4000）

gitbook build：该命令生成网页而不开启服务器
### 3.3 插件
在gitbook init的目录下新建book.json，然后往里填入

```
{
    "plugins": [
        "-lunr", "-search", "search-pro",
        "back-to-top-button",
        "chapter-fold",
        "code",
        "splitter",
        "tbfed-pagefooter",
        "page-treeview",
        "popup",
        "expandable-chapters",
        "-sharing", "sharing-plus",
        "page-toc-button"
    ],
    "pluginsConfig": {
        "tbfed-pagefooter": {
            "copyright": "Copyright &copy qgao 2021-*",
            "modify_label": "该文件修订时间：",
            "modify_format": "YYYY-MM-DD HH:mm:ss"
        },
        "page-treeview": {
            "copyright": "Copyright &#169; qgao 2021-*",
            "minHeaderCount": "2",
            "minHeaderDeep": "2"
        },
        "sharing": {
            "douban": true,
            "facebook": true,
            "google": true,
            "qq": true,
            "twitter": true,
            "weibo": true
        },
        "page-toc-button": {
            "maxTocDepth": 4,
            "minTocSize": 2
        }

    }
}
```

再通过gitbook install安装

## 4 bug
### 4.1 Error: ENOENT: no such file or directory, stat (…) 【version 3.2.3】
用户目录下找到以下文件。

.gitbook\versions\3.2.3\lib\output\website\copyPluginAssets.js

把所有的confirm: true替换为confirm: false

### 4.2 子目录中的md文件内目录格式失效
不要把目录写在文件第1行，第1行无法起效

### 4.3 Template render error … expected variable end
连在一起的两个大括号如

\{\{2\}\}

会把括号里的2当做一个模板变量。如果这个变量不存在，会报错 Template render error: expected variable end。详细的解释看下面：

https://wwshen.gitbooks.io/omooc2py/0MOOC/dakuohao.html

https://www.weihuayi.cn/tools/gitbook.html

https://juejin.im/post/5ce51e126fb9a07ed440d7d0#heading-22

解决方式

把连续的大括号\{\{替换为中间加个空格{ {，把\}\}替换为} }。

### 4.4 gitbook一个进程要占用（至少）两个端口，35729、4000。

同主机启动多个gitbook实例，需要port、lrport各不占用、各不相同。

`gitbook serve --port=4001 --lrport=4101`

## 5 更多同类型工具

* docsify
* 看云文档
* ...