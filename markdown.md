<!-- This page embeds a markdown file from the docsify-themeable website -->

# docsify+Github搭建自己的文档
### docsify是什么？
<font size="4">简单说呢:是一个轻量化的文档生成器,可以说是很轻量级的了
 我们可以用它来搭建一个自己的文档;
</font>

### 参考资料
- [docsify官网](https://docsify.js.org/#/)
- [定制主题](https://jhildenbiddle.github.io/docsify-themeable/#/)
- [这篇文章](https://segmentfault.com/a/1190000017576714)
- [Markdown语法](https://markdown-zh.readthedocs.io/en/latest/)

### 具体步骤
准备环境:

[node.js](https://nodejs.org/en/)

[git](https://git-scm.com/)

>适合有一点Markdown基础和一点git基础以及一点点敢于探索精神的人阅读

1.机器上面要安装node.js环境；如果没有安装的话这里是官网可以下载安装就行了:[node.js](https://nodejs.org/en/)

![](https://raw.githubusercontent.com/VIlboys/img/master/docsify/01.png)

2.在本地新建一个文件夹，然后打开cmd命令行窗口，执行命令安装 docsify-cli 工具，可以方便创建及本地预览文档网站。
```cmd
npm i docsify-cli -g
```
3.通过以下命令来初始化项目
```cmd
docsify init ./docs
```
4.初始化完成后，可以看到有一个`docs`目录下面有几个文件
- `index.html`文件
- `README.md`做为主页内容渲染
- `.nojekyll` 用于阻止 GitHub Pages 会忽略掉下划线开头的文件

### 本地预览项目
进入docs文件夹然后再使用命令:
```cmd
docsify serve
```
或者
```cmd
docsify serve docs
```
出现下面这段话表示已经在`3000`这个端口上面运行了，在浏览器上面预览`http://localhost:3000`这个地址

![](https://raw.githubusercontent.com/VIlboys/img/master/docsify/02.png)

默认的页面样式(我这里是改成中文的了，默认是英文的)：

![](https://raw.githubusercontent.com/VIlboys/img/master/docsify/03.png)

### 添加点东西
如果想添加更多的页面可以参考官网的页面一样，就需要添加更多的md的文件
##### 1.添加封面页
在`docs`文件下新建一个`_coverpage.md`文件记住是带下划线的，然后在里面编写内容
```md
# 我的文档
- 很好用的文档生成器
- 很轻量化

[baidu](http://baidu.com)
```
##### 2.添加更多页面
如果需要创建多个页面，或者需要多级路由的网站，在docsify里也能很容易的实现。例如创建一个guide.md文件，那么对应的路由就是/#/guide。
如果你的目录结构如下：
```
| ./
  -| README.md
  -| guide.md
  -| zh-cn/
    -| README.md
    -| guide.md
```
那么对应的访问页面将是:
```
./README.md        => http://domain.com
./guide.md         => http://domain.com/guide
./zh-cn/README.md  => http://domain.com/zh-cn/
./zh-cn/guide.md   => http://domain.com/zh-cn/guide
```
### 定制侧边栏
1)，首先需要在首页把`loadSidebar`设置为`true`
```html
 <script>
    window.$docsify = {
      loadSidebar :true
    }
  </script>
```

2)，然后在根目录创建`_sidebar.md`文件，内容格式如下：
```
- [首页](/)
- [折腾](markdown)
```
3)，接着在根目录创建`markdown.md`文件，就可以写点内容

### 显示页面目录
1)，在`index.html`里面添加`subMaxLevel`属性
```html
<script>
    window.$docsify = {
      subMaxLevel: 9
    }
  </script>
```


### 网站部署到GitHub
1)，新建一个存储库(名字见名知意就好)

2)，把本地文件上传到Github上面

3)，在存储库的设置里面的`GitHub Pages`选项


##### GitHub Pages 支持从三个地方读取文件:

1、master分支

2、master分支下的docs目录

3、gh-pages分支

1、如果你的文档直接是在项目根目录写的，那么可直接把代码推送到master分支上， GitHub Pages里选择master branch.
2.如果你的文档是在master分支下的docs/目录下编写的，那么可直接把代码推送到master分支上，GitHub Pages里选择master branch/docs folder.
本例子项目是直接在根目录中编写的，所以GitHub Pages里选择master branch的方式部署。
如下图所示:
![](https://raw.githubusercontent.com/VIlboys/img/master/docsify/04.png)

然后等几分钟再访问你的域名地址(记得配置DNS解析)就可以看到你的文档了

结束~~