## 1 前言

在使用 Obsidian 之前，我长时间使用 Notion 作为自己的主力笔记软件，Notion 足够优秀，但是服务一旦挂掉，我们的笔记就会无法访问，当然 Notion 一年可用时间应该有 99.999%，但是不怕一万就怕…，另外 Notion **云端存储**，不能离线阅读，但现在离线的场景其实也基本没有 ，所以离线也只是个伪需求，除非在信号差的地方。最近 Web3 这个概念火起来了，其中一个特点就是去中心化，追随下时代的潮流。

Obsidian 真的适合喜欢折腾的同学，在 Obsdian 我仿佛找到当初在大学给 Android 手机刷 Windows Phone 的感觉，时间过得真快不再是少年，一颗爱折腾的心仍然不变。或许真的有点喜欢折腾笔记软件，大学毕业设计就是开发了一款 Android 版的 PKM，但是源代码已经找不到了，不然得看下当时写的代码有多少槽点 。

文尾附了**我的信息流获取**

## 2 介绍

优势：

- 文本记录，方便迁移，记录各种 [metadata](https://zhida.zhihu.com/search?content_id=557415844&content_type=Answer&match_order=1&q=metadata&zhida_source=entity)
- 笔记版的 vscode，插件市场丰富，够折腾，满足个性化定制

缺点：

- 似乎不支持 Notion 那样子的 Block，不方便拖拽

## 3 使用

我平时除了使用核心的[双链笔记](https://zhida.zhihu.com/search?content_id=557415844&content_type=Answer&match_order=1&q=%E5%8F%8C%E9%93%BE%E7%AC%94%E8%AE%B0&zhida_source=entity)外，还会把他当作个人本地数据库使用，主要利用 Markdown metadata 记录元数据，Templater 方便的复用模版，配合 Dataview table 汇总输出，当然这些加起来其实就是 Notion 的 database（Obsidian 有个类似的插件 DB Folder），而且 Notion 数据记录形式会更加高效，但是 Obsdian 可以输出的不仅仅表格，还是可以使 Charts 和 Heatmap 等图表形式。

记录场景：

- 阅读清单，在读，想读，读过，之前用豆瓣书单
- 影视记录，在看，想看，看过
- 软件订阅，订阅记录，变更记录
- 投资记录，基金，股票，不过这个场景 Beancount 更合适

记录意义：

- 习惯追踪
- 输出年度复盘

### 3.1 同步

利用 git 来支持历史版本，手动 push 到 Github，社区有一个自动定时同步远程仓库的插件，但我觉得没必要，就像自己平时提交代码一样来提交笔记，也可以维护 commit message。同时我也会使用 iCloud 进行多重备份

### 3.2 移动端

Obsidian 有官方移动端，利用 iCloud 来同步，满足自己的查询需求，移动端本身不方便做复杂的编辑，可以记录一些灵感和想法。

### 3.3 一些插件

- Daily notes，基于这个日记 [模版](https://link.zhihu.com/?target=https%3A//dannb.org/blog/2022/obsidian-daily-note-template/) 针对自己做了些改良，现在每天的收获和随机想法都记录在里边
- Dataview，必装插件，像操作数据库一样来维护自己的笔记系统，通过 metadata 建立索引，自定义查询归纳文档，页面内嵌变量，执行自定义 js 脚本
- Templater，模版复用、插入变量，比如结合 [Banners](https://link.zhihu.com/?target=https%3A//github.com/noatpad/obsidian-banners) 随机生成封面图、emojj icon，笔记颜值瞬间增加不少
- Mind Map，实时预览脑图，整理笔记也变成了一种享受
- Tasks，强大的任务管理，还可以生成周期任务
- Calendar，配合 Daily Note 使用，可以以日历的形式显示某天的 TODO 是否完成
- Omnivore，文章可以按天归档，导出文章、高亮和笔记，一款开源 ReadItLater 软件，作为 Readwise Reader 的替代品
- Weread，同步[微信读书](https://zhida.zhihu.com/search?content_id=557415844&content_type=Answer&match_order=1&q=%E5%BE%AE%E4%BF%A1%E8%AF%BB%E4%B9%A6&zhida_source=entity)中书籍元信息、划线和想法
- QuickAdd，快速添加，支持加载 js 脚本，比如调用 API 拉取电影 metadata，和 Templater 多少有点重合，感觉还是 Templater 用起来更舒服一点
- Periodic Note，周期记录，方便记录周报、月报、年报
- Annotator，PDF, EPUB 高亮标注、跳转

### 3.4 Tips

- 文件名以英文或数字开头， Command + O 方便快速打开和检索，不用切换输入法
- 不要在文件夹的组织上花费过多时间，每日的想法和 TODO 记录在 Daily notes，只有十分明确的笔记主题才去专有页面记录
- 尽可能多的去使用双链和标签，使笔记之间产生关联，构建网络，方便回溯
- 不要花费过多的时间折腾插件和主题，打造自己舒服的工作流，专注于写作的内容

## 4 新发现

[obsidian-plugin-stats](https://link.zhihu.com/?target=https%3A//obsidian-plugin-stats.vercel.app/new) new 模块可以看到新的有意思的插件，比如

- [obsidian-habit-calendar](https://link.zhihu.com/?target=https%3A//github.com/hedonihilist/obsidian-habit-calendar) 习惯追踪
- [obsidian-yt-transcript](https://link.zhihu.com/?target=https%3A//github.com/lstrzepek/obsidian-yt-transcript) 抓取 Youtube 转录

## 5 展望

希望有一天 Obsidian 能够内嵌一个类 ChatGPT 的小型本地模型，帮我们回顾笔记，构建知识网络，就像官网宣称的那样真正成为我们的第二大脑。

## 6 参考

1. [基于 Obsidian 的生活记录系统](https://link.zhihu.com/?target=https%3A//diygod.me/obsidian)
2. [我的 Obsidian 使用经验](https://link.zhihu.com/?target=https%3A//catcoding.me/p/obsidian-for-programmer/)
3. [我的笔记法（借助 Zettelkasten 和 Obsidian）](https://link.zhihu.com/?target=https%3A//einverne.github.io/post/2021/01/my-method-to-take-notes-using-zettelkasten-and-obsidian.html)
4. [我的 Obsidian 笔记跨设备同步方案](https://link.zhihu.com/?target=https%3A//einverne.github.io/post/2020/11/obsidian-sync-acrose-devices-solution.html)
5. [awesome-obsidian](https://link.zhihu.com/?target=https%3A//github.com/kmaasrud/awesome-obsidian) 有趣的 Obsidian 插件或者用法
6. [我是如何使用 Logseq 的](https://link.zhihu.com/?target=https%3A//www.youtube.com/watch%3Fv%3DDxoGJBb1mWQ)

* * *

[我的信息流获取](https://link.zhihu.com/?target=https%3A//blog.ww93.fun/post/info/)

![](https://picx.zhimg.com/7c019e8b74832c94b958bf3882da755c_l.jpg?source=1def8aca)

执笔忆红尘​

其实我觉得你好像对这两个软件，有些误解。

![](https://pica.zhimg.com/v2-82654e7c9e2da836cbbec6c268712830_r.jpg?source=1def8aca)

obsidian 是信息管理类软件，notion 块类数据库类，他们两不是二选一的关系，而是互补的一个关系。notion 自己也能跟 obsidian 实现打通。

你完全可以用 Notion 来学习，用 obsidian 来工作。或者你嫌麻烦的话。像我一样，把 notion 直接装进 obsidian 里。

ob 可以做立体式图谱，多层导航，非常奈斯。

![](https://picx.zhimg.com/v2-498296f909c49240cce7d2df8bc05834_l.jpg?source=1def8aca)

qileq

两者侧重点不太一样，也各有优缺点。

Obsidian 适合构建知识体系，积累的想法越多越丰富，未来的思维也会更丰富。可用来当成一个想法的仓库和生成器。在读《卡片笔记写作法》时，会觉得 Obsidian 非常适合这类方法。另外社区提供了丰富的插件，安装完插件后可以用来保存微信阅读笔记、画图、管理日程表、记录时间、看板、整理阅读记录等等，完全可以当成一个 allinone 的工具。缺点就是官方同步需要收费，要用的好需要花时间配置，另外 ai 相关的插件使用起来并不完美。(羡慕 notion ai 中)

Notion 则针对一些日常场景提供了开箱即用的模板，可以用来做项目管理，wiki，协同工作等等，功能也够强大，现在的 notion ai 也非常具有吸引力。缺点就是数据保存在云端，虽然省了同步的问题，但离线时使用就不方便了 (讲道理现在基本都是在线，所以这个并不是很大的缺点，也不用太担心未来二三十年这家公司会倒，自己的笔记因此丢失。。)

总体来说，如果非要 allinone 的话，愿折腾就选 obsidian，毕竟可定制化性更高。不愿折腾的就无脑选 notion 吧。如果喜欢不同场景使用不同工具的话两者配合使用，强烈推荐使用 obsidian 做知识体系管理和数字花园，用 notion 处理其他方面的需求。

[Obsidian 漫游指南 | QiLeq](https://link.zhihu.com/?target=https%3A//qileq.com/tool/obsidian/)
