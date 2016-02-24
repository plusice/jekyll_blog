---
layout: post
title: ［译文］为什么我弃用gulp和grunt而用npm
excerpt: <p>gulp,grunt,npm</p>
---

我知道你们在想什么?什么鬼?!Gulp不是刚刚干掉了Grunt吗?为什么我们不能就这样维持一会呢?我知道,但是...

**我发现Gulp和Grunt是没有必要的抽象,npm script更强大,并且通常更好用**


### 我们先从一个例子开始...

我本来很喜欢用Gulp,但在我的最后一个项目,我在gulpfile文件里写了几百行代码,并且使用了十几个插件.用gulp我废了很大力气整合Webpack,Browsersync,hot reloading,Mocha和其他很多东西.因为一些插件在我的这个场景下,文档不是很完善,有些插件只给我提供了我需要的API的一部分.有一个插件有一个bug,它只会检测很少的文件.另一个插件在输出到命令行的时候把颜色都给去掉了.

这些都是可以解决的问题,**但是我在直接调用这些工具的时候,一个问题也没有出现.**

后面我注意到很多开源的项目都直接使用了npm scripts.我决定退后一步重新评估,我真的需要Gulp吗?结果证明我需要.

我决定在我新的开源项目上使用npm scripts.我只使用了npm scripts,为React应用创建了一个丰富的开发环境和构建程序.很好奇这是什么样的吗?点击这里<a href="https://github.com/coryhouse/react-slingshot">React Slingshot</a>.

很震惊的是,我现在宁愿使用npm scripts而不用Gulp了.以下是原因:


### Gulp和Grunt有什么问题?

用了一段时间后，我发现类似Gulp和Grunt的任务执行工具有三个主要的问题:

1. 依赖插件作者
2. 难以调试
3. 杂乱的文档

我们先来看一下这几个问题

#### 问题1:依赖插件作者
当你使用新技术或者不流行的技术时,可能根本没有相关的插件.即使有插件,它可能是过期的.例如,Babel 6最近发布了.API改变了不少,所以很多Gulp插件就不兼容最新的版本了.使用Gulp的时候,因为我需要的一些插件还没更新,我时常会遇到问题.

Gulp和Grunt遇到问题你通常都要等作者提供更新,或者自己修复.这限制了你使用很多流行工具的最近版本.而另一方面,当我使用npm scripts,我是直接使用工具,而没有额外一层的抽象.

至于插件数量上的对比,谁能跟npm比:

<img src="/img/resource/npm-package-num.png">

#### 问题2:难以调试
使用Grnt和Gulp构建失败时,调试是比较麻烦的.因为你是多了一层额外的抽象,所以有更多的可能引起bug:

1. 是否基础工具出错了?
2. 是否Grunt/Gulp插件出错了?
3. 是否我的配置错了?
4. 我是不是用了不兼容的版本?

使用npm scripts不会出现问题2,并且我发现问题3也很少出现,因为我一般直接调用工具的命令行借口.另外,我直接使用npm,减少了项目中使用到的package,问题4也出现的较少.

#### 问题3:杂乱的文档
我需要的npm core tools文档几乎总是比相应的Grunt和Gulp插件文档好.例如,我使用gulp-eslint时,我要花费很多时间在gulp-eslint文档和ESLint网站间来回切换.用Gulp和Grunt主要的痛点就是:

**了解了工具是不够的,还需要你了解这个工具对应Gulp和Grunt的插件的使用**

大多构建相关的工具提供了清晰,强大和具有完善文档的命令行接口.看一下<a href="http://eslint.org/docs/user-guide/command-line-interface">docs on ESLint's CLI</a>,就是一个很好的例子.我发现在npm scripts中阅读并且实现一个剪短的命令行调用会更清晰,更直接,并且更容易调试(因为没有了一层抽象).

现在我已经列出了所有痛点,问题是,为什么我们认为我们需要像Gulp和Grunt之类的任务执行器.


### 为什么我们忽略了npm的构建功能

我相信Gulp和Grunt这么流行是因为四个误解:

1. 人们认为npm scripts需要很好的掌握命令行的编写
2. 人们认为npm scripts不够强大
3. 人们认为Gulp的流处理对快速构建是必不可少的
4. 人们认为npm scripts不能跨平台

让我们对这四个误解一个个的说明:

#### 误解1:npm scripts需要很好的掌握命令行的编写
要享受使用npm scripts的快感,你不需要掌握很多有关操作系统的命令行.当然,grep,sed,awk和pipe是值得学习的终身受用的技能,但要使用npm scripts你也不用成为Unix或者Windows命令行大师.你也可以使用npm文档中的一千多个脚本来完成工作.

例如,你可能不知道在Unix下这样会删除一个目录:rm -rf.这没有关系,你可以用<a href="https://www.npmjs.com/package/rimraf">rimraf</a>,它的作用是一样的,而且是跨平台的.很多npm包提供的接口,即使没有掌握系统命令行也是很方便使用的.只需要搜索一下你需要的npm包,读一下文档学一下.我之前都是搜索Gulp插件,现在我搜索npm包.顺便提供一个不错的资源:<a href="https://libraries.io/">libraries.io</a>

#### 误解2:npm scripts不够强大
npm scripts其实是很强大的!有一份基于<a href="https://docs.npmjs.com/misc/scripts#description">pre and post hooks</a>的约定:

```
{
  "name": "npm-scripts-example",
  "version": "1.0.0",
  "description": "npm scripts example",
  "scripts": {
    "prebuild": "echo I run before the build script",
    "build": "cross-env NODE_ENV=production webpack",
    "postbuild": "echo I run after the build script"
  }
}
```

你所需要做的就是遵循约定.上面的脚本会根据前缀按顺序执行.prebuild脚本会在build脚本之前执行,它对比build脚本有前缀"pre",而post脚本会在build脚本后面执行因为有前缀"post".所以如果我创建了脚本:prebuild,build和postbuild,当我输入"npm run build"时,它们将会自动按顺序执行.

你也可以通过调用另一个脚本,把一个大任务拆分:

```
{
  "name": "npm-scripts-example",
  "version": "1.0.0",
  "description": "npm scripts example",
  "scripts": {
    "clean": "rimraf ./dist && mkdir dist",
    "prebuild": "npm run clean",
    "build": "cross-env NODE_ENV=production webpack"
  }
}
```

上面的例子prebuild任务调用了clean任务.这允许你把你的脚本拆分成更小,更好命名,更加单一职责的一行代码.

你也可以在一行脚本用&&串行调用多个脚本.上面例子的clean任务,将会顺序执行各个脚本.如果你是一个讨厌Gulp中一长串任务写法的人,这种简单会让你做梦都笑的.

并且如果一个命令变得太复杂了,你也可以调用调用其他的文件:

```
{
  "name": "npm-scripts-example",
  "version": "1.0.0",
  "description": "npm scripts example",
  "scripts": {
    "build": "node build.js"
  }
}
```

上面的例子我在build任务中调用了一个其他文件的脚本.这个脚本会用node执行因此可以调用任何我需要的npm包,并且可以使用到javascript的强大力量!

我就不继续说了,<a href="https://docs.npmjs.com/misc/scripts">主要的特性都在这里</a>.并且,这里还有一个<a href="https://www.pluralsight.com/courses/npm-build-tool-introduction">npm作为构建工具的简短介绍</a>,或者查看<a href="https://github.com/coryhouse/react-slingshot">React Slingshot</a>.

#### 误解3:快速构建一定要用到Gulp的流
Gulp能够快速获得超过Grunt支持的一个原因就是其基于内存操作的stream操作要比Grunt的文件操作要快.但其实要使用stream的力量你完全可以不用Gulp.事实上,使用Unix和Windows的命令行,我们就经常会使用到stream.pipe操作符(|)使一个命令的输出以流的形式作为另一个命令的输入.而重定向操作符(>)将输出定向到一个文件.

例如,在Unix我可以用'grep'检索一个文件的内容并且输出到一个新的文件:

```
grep ‘Cory House’ bigFile.txt > linesThatHaveMyName.txt
```

上面所做的就是流,没有中间的文件被写.(想知道上面的命令怎么跨平台使用吗?那继续读下去...)

在Unix上,你也可以用'&'操作符来同时跑两条命令:

```
npm run script1.js & npm run script2.js
```

上面两个脚本将在同一时间执行,想要同时跨平台,使用<a href="https://www.npmjs.com/package/npm-run-all">npm-run-all</a>.这引出了我们的下一个误解...

#### 误解4:npm脚本不能跨平台执行
很多项目都是在特定的操作系统使用，所以没有跨平台的忧虑。但如果你需要跨平台，npm脚本也能够很好的工作。无数的开源项目就是证明。下面说明是怎么做到的。

你的操作系统命令行执行了npm脚本，所以在Linux和OSX你的npm脚本通过Unix命令行执行。而Windows上，npm脚本通过Windows命令行执行。因此，如果你要你的构建脚本可以在所有平台上执行，你要让Unix和Windows都开心。下面是3个方法：

**方法1:** 使用跨平台的命令，很幸运的是竟然有如此多的跨平台命令，以下是其中一些：
```
&& chain tasks (Run one task after another)
< input file contents to a command
> redirect command output to a file
| redirect command output to another command
```

**方法2:** 使用node packages。你可以使用node packages取代shell命令。例如，使用<a href="https://www.npmjs.com/package/rimraf">rimraf</a>取代“rm -rf”。使用<a href="https://www.npmjs.com/package/cross-env">cross-env</a>来跨平台设置环境变量。在Google，npm或者<a href="https://libraries.io/">lirbraries.io</a>上搜索你所需要的node package，一般都能找到一个可以跨平台的。另外，如果你的命令行调用过长，你可以用分离的脚本调用node packages，像这样：

```
node scriptName.js
```

上面是一个用node跑的简单脚本。如果你仅仅是想用命令行调用一个脚本，你可以不使用.js文件。你可以在你的系统上使用Bash，Python，Ruby，或者Powerell执行任意脚本。

**方法3:** 使用<a href="https://www.npmjs.com/package/shelljs">ShellJS</a>.ShellJS是一个通过node执行Unix命令行的npm包。所以这可以让你在任何平台上面执行Unix命令，包括Windows。

我在<a href="https://github.com/coryhouse/react-slingshot">react slingshot</a>使用了方法1和方法2

### 痛点

不过不可否认的是也存在一些缺点：因为JSON不能添加注释，所以在package.json里不能添加注释。有几个方法可以用来处理这个限制：

1. 脚本写得简洁、单一职责并且命名规范
2. 脚本文档分离（例如写在readme上）
3. 调用一个separate.js文件

我更倾向于第一种。如果你分离好脚本使各脚本单一职责，注释是可以不要的。脚本的名字应该描述其目的，就像那些命名规范的小函数。正如我在<a href="https://www.pluralsight.com/courses/writing-clean-code-humans">Clean Code: Writing Code for Humans</a>中讨论的，单一职责的小函数是不需要注释的。如果我觉得注释是必须的，我会使用方法3，把脚本迁移到一个分离的文件。把<a href="https://github.com/kriasoft/react-starter-kit/blob/master/package.json#L74">React-starter-kit</a>check out下来查看一个简单的例子。

总之，仍然会有可能创建让人很难看懂的又长又臭的命令行参数。而确保npm脚本分离成简洁、单一职责并且命名规范容易理解的小功能，代码审查和不断的重构是一个不错的方法。而如果脚本复杂到真的需要注释，你应该把单一的脚本分离成多个命名规范的脚本，或者抽离到分离的文件。

#### 抽象要恰当

Gulp和Grunt都是我使用过的抽象工具。抽象是有用的，但是也有代价。它们让我们依赖于插件的维护者和文档，并且越来越多的依赖使他们更复杂。我已经觉得我不再需要Gulp和Grunt这样的任务执行器了。

我不是第一个这样建议的人了，下面是一些不错的文章：

- <a href="http://substack.net/task_automation_with_npm_run">Task automation with npm run — James Holliday</a>
- <a href="https://www.youtube.com/watch?v=0RYETb9YVrk">Advanced front-end automation with npm scripts</a>
- <a href="http://blog.keithcirkel.co.uk/how-to-use-npm-as-a-build-tool/">How to use npm as a build tool </a>
- <a href="http://app.pluralsight.com/courses/npm-build-tool-introduction">Introduction to npm as a Build Tool </a>
- <a href="http://gon.to/2015/02/26/gulp-is-awesome-but-do-we-really-need-it/">Gulp is awesome, but do we really need it?</a>
- <a href="http://code.tutsplus.com/courses/npm-scripts-for-build-tooling">NPM Scripts for Build Tooling</a>

