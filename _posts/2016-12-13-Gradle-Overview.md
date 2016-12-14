---
layout: post
title: Gradle Overview
category: Gradle
tags: [Gradle]
no-post-nav: true
---

## Introduction
* [Gradle](https://gradle.org/)是一个通用的项目构建工具(官方说像Ant一样,反正我没用过...所以我不懂),它不单单可以用来构建Java项目,还可以用来构建各种语言相关甚至非语言相关的项目,一切都决定于你想用它来干什么
* [Gradle](https://gradle.org/)是一个遵循约定大于配置的项目构建工具(像[Maven](http://maven.apache.org/)一样,这回我用过,所以就不加**官方说**了),但是它的一起操作都支持自定义.但是为了世界和平,我个人觉得还是尽可能遵循约定会比较好
* [Gradle](https://gradle.org/)支持多项目构建,而且功能非常的强大,真的非常强大,已经不是[Maven](http://maven.apache.org/)可以比拟的了,[Gradle](https://gradle.org/)甚至可以把非同一项目根目录的项目引用到你的项目里面来进行项目构建,虽然这个功能现在只是处于孵化阶段(这个东西叫 Composite Builds 翻译成中文应该是叫混合构建吧).
* [Gradle](https://gradle.org/)拥有非常强大的依赖管理,反正[Maven repository](http://search.maven.org/),[Ivy repository](http://ant.apache.org/ivy/), Local repository,甚至是你随便丢的依赖(包括网络上的),它都可以帮你正确的引入进来.在多项目构建的时候,它还可以直接引入你的项目作为依赖,如果你愿意的话,它还可以把用这个引入的项目来替代你引入的正式发布的依赖
* [Gradle](https://gradle.org/)为你解决了循环依赖噩梦,有很多配置可以让你选择,它默认会为你把依赖提高到你所有引用到的最高版本.(就是说,例如你某个依赖用的1.0版本,另一个用了2.0版本,最后依赖统一比提高到2.0版本)
* Ant被完全融合进了[Gradle](https://gradle.org/)
* [Gradle](https://gradle.org/)也支持插件的功能,而且它的插件不像[Maven](http://maven.apache.org/)一样,[Gradle](https://gradle.org/)本身只是相当于一个框架的存在,所有真正的构建动作都是由指定的插件去完成.而[Maven](http://maven.apache.org/)的插件只是在对[Maven](http://maven.apache.org/)的功能上的扩展.
* [Gradle](https://gradle.org/) 的构建靠的就是一个非常强大的脚本,脚本语言是基于[Groovy](http://groovy-lang.org/)的DSL(Domain Specific Language),所以它也支持所有的[Groovy](http://groovy-lang.org/)语法.它不像[Maven](http://maven.apache.org/)靠的是xml静态配置文件,既然[Gradle](https://gradle.org/)的构建脚本是可编程的,它的灵活性就不用我多说了,最简单的例子就是,[Gradle](https://gradle.org/)的插件可以让用户做到,在指定的动作上面直接添加用户自定义的动作,这不是简单的xml可以做得到的


## The Build Lifecycle
官方文档把这个放在很后面去介绍,我觉这个应该放在这里说,要不然看的人云里雾里(起码要简单的结束一下嘛).总的来说[Gradle](https://gradle.org/)的脚本执行就分2个阶段,配置阶段,和执行阶段,默认情况下,这些阶段都由**build.gradle**和**settings.gradle**这2个脚本文件来完成了( **settings.gradle** 只在多项目构建的时候才有用).下面简单的说一下:

1. Configuration
  * 首先是配置阶段,在这个阶段下所有脚本里面的代码都会被[Gradle](https://gradle.org/)加载,而且执行对应的配置,赋值,初始化等工作
2. Execution
  * 然后这里是重头戏了,[Gradle](https://gradle.org/)一切的执行都是靠**Task**,在完成了所有的初始化工作之后,[Gradle](https://gradle.org/)就会执行用户命令行输入的目标**Task**去完成脚本上的一切执行动作(注意**Task**是分执行部分和配置部分的,这里只是简介一下[Gradle](https://gradle.org/)我就不赘述了,有兴趣的请自行查阅[Gradle用户文档](https://docs.gradle.org/current/userguide),不谢 :sunglasses:),然后你想做的一切,就会为你完成了.
3. 在构建的时候,[Gradle](https://gradle.org/)会生成很多对象,然后你可以在你的脚本里调用这些对象的属性.

## Groovy Quickstart
反正用[Gradle](https://gradle.org/)你就跑不掉要基本理解下[Groovy](http://groovy-lang.org/),基本的语法你还是得知道,[Gradle](https://gradle.org/)的官方文档有这个一个章节,[Chapter 53. Groovy Quickstart](https://docs.gradle.org/current/userguide/tutorial_groovy_projects.html),反正也可以看一下.值得注意的是,[Gradle](https://gradle.org/)大量地使用了[Groovy](http://groovy-lang.org/)闭包的[Delegation](http://groovy-lang.org/closures.html#closure-owner)机制和[Bean](http://groovy-lang.org/objectorientation.html)的概念([也可以看看这里的讲解](http://www.cnblogs.com/davenkin/p/gradle-learning-3.html)),先理解这些,看[Gradle](https://gradle.org/)的脚本就不会觉得懵逼 :flushed:

## About Task
**Task**是[Gradle](https://gradle.org/)灵魂一样的存在,所有真正的执行动作都是有**Task**去完成,例如:

```
task hello {
  doLast {
      println 'Hello world!'
  }
}
```
然后执行 **gradle -q hello** 就会输出

```
> gradle -q hello
Hello world!
```
就会输出 **'Hello world!'** 了
这意味着什么?这个是脚本语言,你可以自己写一个**Task**然后把你的代码填进去,然后命令行执行一下,你就可以做 做 **What ever you want!!!!**

然后这个**Task**还可以继承,(好像可以复写,但是这个比较复杂了)

```
task taskX(dependsOn: 'taskY') {
  doLast {
      println 'taskX'
  }
}
task taskY {
  doLast {
      println 'taskY'
  }
}
```
然后执行 **gradle -q taskX** 就会输出

```
> gradle -q taskX
taskY
taskX
```

看这样就会先执行taskY再执行taskX,现在的例子看起来简单,但是当父**Task**的功能非常通用,强大的时候,想要继承实现这个父**Task**功能就会变的非常简单了.来再开个例子

```
task copyDocs(type: Copy) {
    from 'src/main/doc'
    into 'build/target/doc'
}
```
就这样,执行一下**gradle -q taskX**,**src/main/doc** 里面的文件就会被复制到 **build/target/doc**里面了,是不是非常简单!

## About Plugin
然后牛逼的东西来了,因为[Gradle](https://gradle.org/)有上面累述的特性,它的插件就变得异常的强大了,例如你本来构建的是**Java**,换个插件你就可以构建**golang**项目了,当然这里我说的夸张了,还是要改挺多东西的.举个栗子:
默认的java插件在打包的时候是不会把依赖都打进jar里面的,然后我加个插件(这个插件叫[shadow](https://plugins.gradle.org/plugin/com.github.johnrengelman.shadow))

```
plugins {
  id 'java'
  id "com.github.johnrengelman.shadow" version "1.2.4"
}

shadowJar {
  manifest {
      attributes 'Main-Class': 'app.SingleServerMain'
  }
}
```
然后执行 **gradle -q shadowJar -x Jar** 依赖就全打进jar里面了.可能有同学说,这和[Maven](http://maven.apache.org/)有什么区别.那就再举个简单的栗子

```
plugins {
  id 'java'
  id "com.github.johnrengelman.shadow" version "1.2.4"
}

shadowJar {
  manifest {
      attributes 'Main-Class': 'app.SingleServerMain'
  }
}

//只加了这个
shadowJar.doLast {
        println 'pack finished!!!!!'
    }
```
上面我只加了一句,但是[Maven](http://maven.apache.org/)就绝对做不到了,更牛逼的功能,我在这就不累述了.
啊,对了,[Gradle](https://gradle.org/)还有它自己的插件库,找插件就再也不用妈妈担心了[Gradle Plugins](https://plugins.gradle.org)


## Finally
  上个对比图[Gradle](https://gradle.org/) vs [Maven](http://maven.apache.org/)
  ![gradle_vs_maven](/assets/images/gradle_vs_maven.png)实现同样的项目构建功能,[Gradle](https://gradle.org/) 194行,[Maven](http://maven.apache.org/) 506行,[Gradle](https://gradle.org/)完胜 :v:
