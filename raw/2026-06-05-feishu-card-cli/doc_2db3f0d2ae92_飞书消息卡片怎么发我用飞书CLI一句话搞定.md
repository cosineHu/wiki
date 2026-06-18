![cover_image](https://mmbiz.qpic.cn/mmbiz_jpg/62UPwaOVsFp9Yp27qRYNZpsW7rnLJ8ic2m9vDbYEE7pibHDaNqIdlu7ZiaL207Ft8Svfb45586pqxO4zia9sQJqgAmZJpXdUtSKTZeWABlxw0LY/0?wx_fmt=jpeg)

#  飞书消息卡片怎么发？我用飞书 CLI 一句话搞定

原创  万涂幻象  万涂幻象  [ 李祥瑞 ](javascript:void\(0\);)

_2026年05月27日 17:39_ __ _ _ _ _ _ 山西  _

在小说阅读器读本章

去阅读

万涂幻象 MAGAZINE  VOL.242 · 2026.05.27  ISSUE FEATURE  飞书CLI × 飞书消息卡片 × Claude
code

在 Claude Code 里跟 AI 说一句话，它自己把一张好看的飞书消息卡片发到群里

  
  
  

我是祥瑞，万涂幻象多维表格社区的主理人，一个深耕飞书多维表格的 AI 落地实践者。

昨天傍晚，我在飞书一个 Demo Day 群里发了张卡片，推我们公众号刚发的那篇《如何让 Obsidian 成为 Claude Code 的大脑，让你的
Agent 越用越聪明》。

![](https://mmbiz.qpic.cn/mmbiz_png/62UPwaOVsFrKPYc6YRflukibpdQu5IBmf7Y2mY4RGOiadYBfY1KFW8W7bqtGHhVlKE1sib75xMQvKfRiaehcLgrDb6eydibz3kw4TvKFtfDHia8Gs/640?wx_fmt=png&from=appmsg)

《如何让 Obsidian 成为 Claude Code 的大脑，让你的 Agent 越用越聪明》公众号封面

不到一小时，一个群友冒出来问：“这种卡片消息是怎么发的？也是通过机器人 + 飞书 CLI 吗？”

今天上午同一个群里，飞书 CLI 官方的同学也发了一张“周三 Ship Note”产品周更卡，下午两点又有人追问：“这个卡片是怎么弄得。”

两张完全不同来源的卡片，24 小时之内被两个人问了同一个问题：怎么做的。

为什么大家会本能地追问？答案不复杂。卡片比纯文本消息直观、好看太多。一张排版好的卡片往群里一推，配图、标题、按钮一应俱全，看着比一段文字加一个链接体面得多。视觉上的高低差摆在那里，看到的人本能就会想：这种东西我也能发出来吗？

![](https://mmbiz.qpic.cn/sz_mmbiz_png/62UPwaOVsFqTnBqdmMzUib1k9DvqXib5ndPYqyDMuhNZibKwfUzLBb8laCYetjQtyymPhorIHsQYEg2B2bvZVVx0iaGUtTvtRvicLjfHp173enP8/640?wx_fmt=png&from=appmsg)

5/26 19:35 我在 Demo Day 群发的那张 mp-card 公众号文章更新卡

两次都有热心的群友直接甩了  ` open.feishu.cn/cardkit  ` 的链接，飞书官方的卡片搭建工具，可视化拖拽那种。

我自己走的不是搭建工具这条路，走的是飞书
CLI。今天这篇把整条路从头拆给你看，先说清楚飞书卡片到底是个啥，再说怎么搭模板、怎么发卡、怎么把它封装成一句话指令。

万涂幻象  
01  卡片不是普通消息

要讲清楚为什么我们要花心思做卡片，得先讲清楚  飞书卡片和普通消息有什么区别  。

飞书群里你日常发的消息，大多是  文本消息  ，一段文字、几个 @、一两个链接，发完就静止了。

飞书卡片  是飞书消息体系里另一个类型，官方叫 interactive message，互动消息。它跟普通消息平级，但  结构和能力完全不一样  。

卡片的本质，是把图文内容做成  结构化的数据  。header、按钮、分栏、图表这些零件按规则拼起来，再由飞书客户端在
PC、手机、网页端按统一样式渲染。同一份卡片能复用到聊天消息、群置顶、链接预览不同场景。这让卡片不再只是一条消息，而是一个能嵌进业务流的  轻量交互组件
。

一张卡片可以做这些事：

●  排版  ：分栏、分块、分列，像一张小海报或一张小图文  ●  配图  ：顶部横幅、内嵌图标、缩略图  ●  按钮  ：可点击，点了之后可以打开
URL、提交表单、触发后端回调  ●  表单  ：输入框、单选多选、文件上传，让用户在卡片里直接交互  ●  图表
：直接在卡片里画柱状图、折线图、饼图，把数据可视化嵌进消息  ●  流式更新  ：发出去之后还能继续改，做直播弹幕、AI 流式回答都靠它
![](https://mmbiz.qpic.cn/mmbiz_png/62UPwaOVsFr9x7AOyBmlaoibDiaPIXic4zKyfru2EiaWaBVgl4vbdO5GdMKgmR7FciaLiaYcwA97mAqCxAWw43vvBs4v1YRxdtYCXxicuTkHNwACqc/640?wx_fmt=png&from=appmsg)

上方是一句普通文本消息，下方是飞书互动卡片：路线、车型、价格、按钮一应俱全

这张图来自我们之前写过的那篇《当龙虾学会打车，在飞书里说一句话车就来了》。同样一个意思“我想叫个车”，纯文字消息只能把需求扔出去，卡片把路线、车型、价格、操作按钮全打包成一个可点的整体。

直观一点说：  普通消息是发一句话过去，卡片是发一张“小海报”过去  。海报上有图、有标题、有按钮，按钮还能点。

飞书卡片的常见用法大致分三类。

一、单向推送类  ：公众号文章卡、直播预告卡、活动通知卡。重点是好看、信息密度高、按钮直接引导动作。  这是我们今天讲的主线  。

![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/62UPwaOVsFpc0VgOqiaJVtVXKvGlYlxndm46pWJ5nERlJwyRzicqTeGiaeqvKMm1qqibXISf4rbkPwPeARfLB6dD8Kmm6gbsfpQiakJqQ2bBNOjc/640?wx_fmt=jpeg&from=appmsg)

万涂幻象社区第一场直播预告卡：浅蓝 header + 直播主题大图 + 时间形式 + 两件事预告 + 一键加入直播按钮

二、交互式工作流类  ：审批卡、问卷卡、确认卡。点按钮就能审批，填表单就能交问卷。

![](https://mmbiz.qpic.cn/mmbiz_png/62UPwaOVsFpvKPsibLYeqJvk3E2QwFkcVZhMw6dPEasbfDibEvXhvcDVgkU9ZcuicdIupvXI7qbDoEG6k1wv5cOUJFZ4ibw28qymUjQqujhq3nM/640?wx_fmt=png&from=appmsg)

龙虾打车的“等待司机接单”卡片，底部“刷新状态 / 取消订单”两个按钮就是典型的交互入口

三、动态刷新类  ：和 AI 机器人聊天时的流式回答、对话过程中卡片组件的实时增删改。卡片发出去之后内容还能继续变。

![](https://mmbiz.qpic.cn/mmbiz_png/62UPwaOVsFp2gUZK7t4au2JLroykNAVm3dPEMN77TCbgA3v2Uz5p8x92qHAocicbOTKZJBn27wibgRHf3XN09M5UkplkibjNmStskmrpLiaeeGU/640?wx_fmt=png&from=appmsg)

同一张卡片下一秒就变了：司机已接单，姓名、车型、车牌、电话、预计到达时间全部刷出来

理解了卡片的能力边界，再回头看你日常在群里发的纯文本消息，会发现很多场景其实用卡片更合适：通知、推送、约会、点选、汇报，这些都比纯文字加一堆链接体面得多。

万涂幻象  
02  为什么不用搭建工具

飞书官方有一个卡片搭建工具，URL 在  ` open.feishu.cn/cardkit  ` ，可视化拖拽，新手友好，拖一拖就能出一张卡片。

但它有几个场景顶不住。

第一，  重复发卡  。万涂幻象一周要发的卡片不止一张。公众号文章一发就要推一张更新卡，每周五晚的社区共学直播一录完就要推一张回放卡，飞书多维表格 57
课系统课程时不时要推一张课程介绍卡。每张都进搭建工具拖一遍，太重。

第二，  跨群分发  。万涂幻象旗下不止一个群，主社群之外还有制造业群、教育群、IT 互联网细分群，一张卡片得发 N
次。搭建工具搭完还得手工挨个发，频率一高就疯。

第三，  AI 自动填充  。卡片要让 Claude 自动抓公众号标题、自动抓首段做副标题、自动上传封面图，搭建工具是 UI 拖拽，AI 接不进去。

![](https://mmbiz.qpic.cn/mmbiz_png/62UPwaOVsFoBpKcuEpyEoIAhic9kZEE8y9dtHRKSJQDQ1icsP9EjffeoSbugO4tiahjd5ExXfvdq5x1ic3A9blLSlicZM2BaD04SnBLfSiagCJib7w/640?wx_fmt=png&from=appmsg)

飞书卡片搭建工具内部 UI：左侧组件库 + 中间画布 + 右侧配置面板

CLI 这条路解决的就是这三件事：  模板复用、批量分发、AI 友好  。

万涂幻象  
03  飞书卡片三件套  

跑通前提  ：装好 lark-cli。装法在前阵子那篇《飞书 CLI 昨天下午破万星，Agent 能替你在飞书干 100 多件事》里详细写过，一句  `
npx @larksuite/cli@latest install  ` 自动适配 macOS、Linux、Windows 装完，再跑  ` lark-
cli auth login  ` 扫码登录飞书账号。能用  ` lark-cli im +chat-list  ` 列出你能看见的群，就算齐活。

飞书 CLI 是飞书官方开源的命令行工具，仓库地址  ` github.com/larksuite/cli  ` ，当前 1.0.41
版本，覆盖飞书几乎全部 OpenAPI：IM、文档、多维表格、日历、审批、OKR、妙记、白板都包了。

做一张卡片要的就三件套。

第一件：卡片 JSON 模板，用 v2 schema。

飞书互动卡片是用 JSON 描述的。最外层是  ` schema: "2.0"  ` ，下面是  ` header  ` 顶部条和  `
body.elements  ` 内容块数组。常用的 element 类型有  ` img  ` 图片、  ` column_set  ` 分栏、  `
markdown  ` 富文本、  ` button  ` 按钮。

最小骨架长这样：

  

{

"schema": "2.0",

"header": {

"template": "wathet",

"title": {"tag": "plain_text", "content": "万涂幻象 · 文章更新"},

"subtitle": {"tag": "plain_text", "content": "{{DATE}} 更新"}

},

"body": {

"elements": [

{"tag": "img", "img_key": "{{COVER_KEY}}"},

{"tag": "column_set", "background_style": "grey", "columns": [...]},

{"tag": "button", "type": "primary_filled", "text": {"content": "阅读全文"}, ...}

]

}

}

` template: "wathet"  ` 是浅蓝色调，飞书内置十几种颜色板供你选。

第二件：image_key 上传。

卡片里的图片  不能直接写 URL  ，必须先把图传到飞书服务器换一个  ` image_key  ` 。一行命令：

  

lark-cli im images create --as bot \

\--file image=@/path/to/cover.png \

\--data '{"image_type":"message"}'

返回的  ` image_key  ` 长得像  ` img_v3_0211n_xxx  ` ，把它塞进模板的  ` img_key  ` 字段就行。

第三件：messages-send 推送。

模板填好之后，发卡就一行命令：

  

lark-cli im +messages-send \

\--chat-id oc_xxx \

\--msg-type interactive \

\--content "$(cat card.json)" --as bot

发完返回一个  ` message_id  ` ，飞书群里一两秒后那张卡片就出现。

![](https://mmbiz.qpic.cn/mmbiz_png/62UPwaOVsFpOpYAMOV9DXeEludgpry1J93Imr5vuurAeBshA3HeetNUQLT4GbBL7PL2TicR4honf0KInj8XVzooIV78LocetDeJHgmxeZCCU/640?wx_fmt=png&from=appmsg)

lark-cli 三步发卡：上传封面 → 替换变量 → 推送互动卡片

到这一步，“  用飞书 CLI 发一张卡片  ”就跑通了。但只跑通这一步，跟搭建工具比没多少优势，重点在下面两节。

万涂幻象  
04  把模板抠出 6 个变量

让 CLI 这条路真正比搭建工具香的关键，是  把卡片设计稳定下来，抠出最小变量集  。

以我们用的公众号文章推送卡为例，内部叫 mp-card，每次发新卡要填的只有 6 个变量：

变量  |  是什么  
---|---  
` {{TITLE}}  ` |  文章标题，从公众号 URL 自动抓  
` {{SUBTITLE}}  ` |  副标题钩子，首段第一句，抓不到让 AI 写一句  
` {{COVER_KEY}}  ` |  封面图的 image_key，本地图自动上传换 key  
` {{MP_URL}}  ` |  公众号文章链接  
` {{WIKI_URL}}  ` |  飞书知识库该文章节点链接  
` {{DATE}}  ` |  当天日期  
  
剩下的浅蓝 header、灰底卡、按钮颜色、按钮文案、入群链接、社区数据，全是  写死的锚点  。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/62UPwaOVsFrzWcRaH0Eias81pLMmZibhoI19KqVvQQibtRrC5yBBzgiazTLVdfqwWlk2JL5UHvWUyxMyiafiaMtG58sqoD5Dfvh1qn9nWC0FIavf4/640?wx_fmt=png&from=appmsg)

左侧 JSON 模板骨架，右侧 6 个变量表，紫色 {{VAR}} 一一对应

这一步是整个方案的精髓。我们这边迭代的时候试过不少方向：header 标题试过“公众号更新”“实践文章”最后定“文章更新”，subtitle
试过放标签串最后定放日期，副按钮文案试过“飞书知识库”最后改成“万涂幻象开源知识库”。这些都是  品牌资产  ，定下来就不再动。

变量管“每次都要换的内容”，锚点管“品牌一致性”  。

抠完这一步，发卡的工作流就只剩三件事：

01  收齐 6 个变量  02  用  ` sed  ` 或 Python 把  ` {{VAR}}  ` 替换进模板  03  跑一遍  `
messages-send  `

不到 30 行 shell 脚本就能搞定。

万涂幻象  
05  跨群分发一行 for

模板和变量都定下来之后，跨群分发就是一行 for 循环：

  

for CHAT in oc_主社群 oc_制造业群 oc_教育群 oc_IT互联网群; do

lark-cli im +messages-send \

\--chat-id "$CHAT" \

\--msg-type interactive \

\--content "$(cat card.json)" --as bot

sleep 1

done

` sleep 1  ` 是为了避免触发飞书的频控。一张卡片打到 N 个群，几秒搞定。

群的  ` chat_id  ` 也是 CLI 拿：

  

lark-cli im +chat-search --query "万涂幻象"

返回所有名字带“万涂幻象”的群，把里面的  ` chat_id  ` 抠出来塞进 for 循环就行。

到这一步，“  用飞书 CLI 高频发卡 + 跨群分发  ”就完整跑通了。

万涂幻象  
06  封一层 skill

教程到上面其实已经讲完了，但我想分享最关键的一步：  把这套流程封装成 Claude Code 的 skill，把命令行藏在自然语言后面  。

说白了就一句话：我做飞书卡片，只需要跟 Claude 说"帮我做张飞书卡片"，剩下的它自己调飞书 CLI 跑通  。

不过在封 skill 之前，得先把 JSON 模板调到自己满意。这一步  全程也是飞书 CLI 撑起来的  。

准确说，是在 Claude Code 里用自然语言指挥 Claude 去跑 lark-cli，让它替我改 JSON 模板、用 dry-run 发到飞书 IM
看真机效果。我说一句“header 颜色再淡一点”，Claude 改  ` template  ` 字段，跑一行  ` lark-cli im
+messages-send  ` 把新版本发到我个人 IM，我刷一下飞书就看到效果。整个过程我嘴上说话，Claude 手上动 JSON 和命令行。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/62UPwaOVsFp6e7hJmxz1I7dqWN3HK0qPK6lM40icbm15RH0SkXeZKenIDhnxb2NynnJibpJ81AHAk1CeicNy2SfIVxwV9XPUFRlDq1ZSqFGXWE/640?wx_fmt=png&from=appmsg)

Claude Code 里用自然语言指挥 Claude 调 lark-cli 做卡片迭代：说一句话 → Claude 改 JSON → dry-run
真机预览

先简单说一句  为什么是 JSON  。前面讲过卡片的本质是结构化数据，这套结构在飞书卡片里用 JSON 来表达，跟 Slack Block
Kit、企业微信、钉钉那些卡片消息一样，都是 web/API 时代的通用做法。结构化的数据天然适合程序生成、批量处理，也天然适合让 AI
直接改写。这是我们今天能用大白话调卡片的根本前提。

但好消息是：  你做卡片完全不需要懂 JSON  。

我调 mp-card 的真实场景是这样的。

我跟 Claude 说，这个 header 浅蓝色再淡一点。它去改  ` template  ` 字段。

我说，标题加粗，副标题不要加粗。它去改  ` weight  ` 字段。

我说，底部那个灰底太突兀了，换成白底加一条细横线。它把  ` background_style: grey  ` 删掉，加个  ` divider  `
块。

我说，发到我自己看下。它跑一行  ` lark-cli im +messages-send  ` 命令 dry-run 到我个人 IM。

我说，对了，就这样。

整个循环里，  我不写一行 JSON，不点一次菜单，全程是口语化指令  。Claude 改 JSON，CLI dry-run
真机预览，我看效果继续说话。几秒钟一个回合，几个回合就能定稿一个细节。

mp-card 这套 14 版迭代下来的设计语言，靠的就是 CLI 这条  写 JSON → 真机预览 → 改 JSON
的快循环。我自己一个人都能跑通，不需要前端、不需要设计稿、不需要任何 UI 工具。

模板稳定之后，再把它封装成 Claude Code 的 skill。

我把 mp-card 这套方案沉淀进了  ` ~/.claude/skills/mp-card/  ` ，里面就两份核心文件：

●  ` SKILL.md  ` ：触发场景、6 个变量来源、固定锚点、执行流程  ●  ` references/card_template.json
` ：完整的卡片 JSON 模板

封装完之后，我发一张公众号卡片的全部口令是这样的：

  

发公众号卡片，文章链接 https://mp.weixin.qq.com/s/xxx，封面图 ./cover.png，知识库链接
https://vantasma.feishu.cn/wiki/xxx

就这一句。

Claude Code 接住之后会自动做完后面所有事：

●  用 defuddle 抓公众号 URL，取标题、取首段  ●  用  ` lark-cli im images create  ` 上传封面拿
image_key  ●  把 6 个变量塞进模板  ●  dry-run 先发到我个人 IM 让我看预览  ●  我说“OK 发主社群”，它再发到目标群
![](https://mmbiz.qpic.cn/sz_mmbiz_png/62UPwaOVsFqLVBICrNtNfzaevEvFtzooQppYtFVkaoQiaF7VypHsdNhwUue7c0SMqVcXnYG1X2OZniaqPD2ZXJvSCTTSUiaJRibmFNKOdC5QW2o/640?wx_fmt=png&from=appmsg)

~/.claude/skills/ 下的三个卡片 skill：mp-card / live-card / lark-shared，共用一套设计语言

同样的方法我们封了三个 skill：

●  mp-card  ：公众号文章推送卡  ●  live-card  ：每周五晚共学直播回放卡  ●  课程推介卡模板  ：飞书多维表格 57
课系统课程介绍卡

三张卡片共用一套设计语言：  wathet 浅蓝 header + 灰底卡 + primary_filled 主按钮 + 双副按钮 + 居中小字脚注 +
全禁 emoji  。

这就是我管它叫  言出法随  的原因。底层是飞书 CLI，中层是模板加变量，上层是 Claude Code
接住自然语言。你说一句话，它走完三层落地成一张卡片。

  
写在最后  

回到那两个追问卡片做法的朋友。

答案三句话。

底层用飞书 CLI，仓库  ` larksuite/cli  ` ，拼 JSON、上传图片、调 messages-send。

中层把卡片设计稳定下来，抠出最小变量集，剩下全做成写死的品牌锚点。

上层把整套流程封成 Claude Code 的 skill，让自然语言能触发。

我自己用的时候，这三条路其实是叠着用的。搭建工具偶尔做一次性的复杂卡片，CLI 是日常发卡的主力，skill
把高频场景封进去之后我连命令都不敲了。不是哪条路替代哪条路，是按场景挑。

万涂幻象其实不只是教大家用飞书多维表格。这两年我们越做越发现，飞书多维表格只是底座，真正卡住大家的是“AI
怎么真的落到自己的业务里”。所以现在更新的内容也越来越宽，从多维表格本身，到 vibe coding、飞书 CLI、Claude Code skill、AI
Agent 工作流，把这些工具怎么真正塞进日常协作里都写。

我们做的事，是把这些藏在工作流深处的“第十次”捞出来给你看。一次卡片做法只是一个切面，下面还有更多 AI 落地的小招数等着拆。

如果你也想把社群通知、直播回放、公众号文章推送封成一句话指令，可以先从一张固定卡片开始。把你最高频要发的那一张设计稳定，抠出几个变量，剩下全做成写死的锚点，再用
lark-cli 跑通 dry-run 预览。这一张跑顺了，其他都是同一套路。

你跟 AI 聊的每一句话，迟早都会变成它的肌肉。

万涂幻象 MAGAZINE  VOL.242 · 2026.05.27

预览时标签不可点

微信扫一扫  
关注该公众号



微信扫一扫  
使用小程序

****



****



****



×  分析

__

![作者头像](http://mmbiz.qpic.cn/mmbiz_png/62UPwaOVsFoL4MeM8I9oL4SFvRByaACTB7SSdwgLrfSAQIfT8VGicwYgFhS5ywbUaVfDU7gPo40ycQuOHpw0oWKiaP2uWrxyc3DmTTm7TaOds/0?wx_fmt=png)

微信扫一扫可打开此内容，  
使用完整服务

：  ，  ，  ，  ，  ，  ，  ，  ，  ，  ，  ，  ，  。  视频  小程序  赞  ，轻点两下取消赞  在看  ，轻点两下取消在看
分享  留言  收藏  听过

