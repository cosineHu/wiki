![cover_image](https://mmbiz.qpic.cn/sz_mmbiz_jpg/naQN5RgSZKGAOKzE3PGbFeticNwVQiaxroicPIb1dXmTyic2V6XG4gc0QHOy6iav7ptQpSxnLaK2WpSHjBddqvq4fphdgma2LHh1dBlVzYbkMUlQ/0?wx_fmt=jpeg)

#  Hermes 飞书流式卡片：告别 Agent 刷屏，一张卡片搞定一切

AI松鼠派  AI松鼠派  [ AI松鼠派 ](javascript:void\(0\);)

_2026年06月14日 09:08_ __ _ _ _ _ _ 安徽  _

在小说阅读器读本章

去阅读

  

> 把 Hermes Agent 在飞书里的回复，从一堆灰色散装消息变成一张持续更新的交互式卡片。

##  为什么需要它？

用 Hermes Agent 接入飞书后，你大概率遇到过这些问题：

![](https://mmbiz.qpic.cn/mmbiz_png/naQN5RgSZKHGibVXOdiaOGwWs9zb7rzte7rh2pibDwkzYNg7250X3aIkgan8pibsL53mze8viayjTfLku6wuSTyEVa4sYlcrImj1PnMKVHmCOIY8/640?wx_fmt=png&from=appmsg)

  

  * ** 刷屏  ** ：一次对话下来，思考过程、工具调用、最终答案拆成十几条灰色消息，上下文全断。 
  * ** 漏字乱序  ** ：流式回复经常丢字、错序，甚至完成后又冒出一条原生消息。 
  * ** 长内容变乱码  ** ：Markdown 表格和代码块被飞书渲染成 raw text，完全没法看。 
  * ** 交互靠手打  ** ：Agent 需要你授权或选择时，得手动输入编号，没有按钮。 
  * ** 排查靠猜  ** ：sidecar 或 hook 出问题时，不知道哪里坏了。 

** hermes-feishu-streaming-card  ** 就是为了解决这些痛点而生的。

##  它做了什么？

一句话：  ** 把 Hermes 的所有事件（思考、答案、工具调用、授权确认、运行统计）聚合到同一张飞书卡片里，实时更新。  **

****

核心能力：

能力  |  说明  
---|---  
流式卡片  |  ` thinking.delta  ` 、  ` answer.delta  ` 、  ` tool.updated  `
持续更新同一张卡片  
卡片内交互  |  approval / clarify 渲染成飞书按钮，点击后原任务继续执行  
长内容保护  |  表格按结构边界拆分、代码块保持完整 fence，不会变 raw markdown  
多 Bot / 多 Profile  |  支持多飞书机器人、群聊绑定、per-bot 标题和路由诊断  
故障隔离  |  hook fail-open，sidecar 挂了不会影响 Hermes 原生文本  
运维工具  |  ` doctor  ` 诊断、  ` start/stop/status  ` 进程管理、  ` restore/uninstall  `
安全回退  
  
##  架构：Sidecar-only

插件采用  ** sidecar-only  ** 架构，对 Hermes 侵入极小：

    
    
    Hermes Gateway  
      └─ gateway/run.py 中的轻量 hook（fail-open）  
           └─ HTTP POST /events ──→  Sidecar Server  
                                       ├─ CardSession 状态机  
                                       ├─ render_card() 卡片渲染  
                                       ├─ Feishu CardKit HTTP Client  
                                       ├─ 节流、合并、重试、锁  
                                       └─ /health 指标端点  
    

Hermes hook 只负责把事件转发给 sidecar，所有飞书发送、更新、重试逻辑都在 sidecar 内独立运行。sidecar 挂了？hook
自动跳过，Hermes 原生文本照常投递。

##  安装：一行命令

** macOS / Linux：  **

    
    
    curl -fsSL https://raw.githubusercontent.com/baileyh8/hermes-feishu-streaming-card/main/install.sh | bash  
    

** Windows PowerShell：  **

    
    
    irm https://raw.githubusercontent.com/baileyh8/hermes-feishu-streaming-card/main/install.ps1 | iex  
    

安装脚本会自动完成：安装 pip 包 → 读取/提示飞书凭据 → 写入  ` ~/.hermes/.env  ` → 调用整合安装器检测 Hermes 版本
→ 安装 hook → 启动 sidecar → 健康检查。

如果凭据已存在，全程无交互。

##  安装后验证

    
    
    # 查看 sidecar 状态  
    python3 -m hermes_feishu_card.cli status --config ~/.hermes/config.yaml  
      
    # 发送真实飞书 smoke 测试卡片  
    python3 -m hermes_feishu_card.cli smoke-feishu-card --config ~/.hermes/config.yaml --chat-id oc_xxx  
      
    # 诊断 Hermes 兼容性  
    python3 -m hermes_feishu_card.cli doctor --hermes-dir ~/.hermes/hermes-agent  
    

确保 Hermes  ` config.yaml  ` 中启用流式：

    
    
    streaming:  
      enabled: true  
      transport: edit  
    

##  Hermes自主安装

当然你也可以像我一样使用 Hermes 自主安装。

![](https://mmbiz.qpic.cn/mmbiz_png/naQN5RgSZKHpAfn2QFp91RyJ9yMiavFOmDwyibrT43vOy7OozU2elNY8KA8XiaTOT1x9f6gFlsKXanJDQ4KmJZWf8Jx1xn5WElk5Ey7ZWWibULc/640?wx_fmt=png&from=appmsg)

  

最后祝大家玩的开心。

  

[ Hermes Agent 到底能干什么？
](https://mp.weixin.qq.com/s?__biz=MzYzNzkyNjQ1OA==&mid=2247483735&idx=1&sn=d30f5ce2f1e6a71452694524994f5461&scene=21#wechat_redirect)

[ 聊聊我使用 Hermes 2 周后的感受
](https://mp.weixin.qq.com/s?__biz=MzYzNzkyNjQ1OA==&mid=2247483728&idx=1&sn=61e3b134ca8acd5f888963fa59dfe4a4&scene=21#wechat_redirect)

[ 爱马仕助手已来，openclaw 拜拜
](https://mp.weixin.qq.com/s?__biz=MzYzNzkyNjQ1OA==&mid=2247483686&idx=1&sn=25a630d7fe14bad23c01d6e0dba19195&scene=21#wechat_redirect)

  

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

![作者头像](http://mmbiz.qpic.cn/sz_mmbiz_png/naQN5RgSZKHWlP9SD8UicEhYwQmFiaMDOTuEo42tbXgqOfV3WANwyUXONuU15Kn9ysNc97kxZGyTYXTYFvut6eAWS0LcIiaQbDx8IJJZ6z9tNM/0?wx_fmt=png)

微信扫一扫可打开此内容，  
使用完整服务

：  ，  ，  ，  ，  ，  ，  ，  ，  ，  ，  ，  ，  。  视频  小程序  赞  ，轻点两下取消赞  在看  ，轻点两下取消在看
分享  留言  收藏  听过

