---
ID: 2434
post_title: 一种现代的身份认证设计思路
author: MFA MFA
post_excerpt: ""
layout: post
permalink: 'http://www.forgerockcn.com/2018/06/29/%e4%b8%80%e7%a7%8d%e7%8e%b0%e4%bb%a3%e7%9a%84%e8%ba%ab%e4%bb%bd%e8%ae%a4%e8%af%81%e8%ae%be%e8%ae%a1%e6%80%9d%e8%b7%af/'
published: true
post_date: 2018-06-29 10:11:48
tags:
  - MFA OTP token 令牌
categories:
  - IAM
---
# 一种现代的身份认证设计思路

<span style="padding-left:28px"></span> 密码已死. 密码万岁!我已经不记得我看了多少文章和博客，关于弱点, 管理, 灵活性, 安全, 不安全和所有的涉及到用户身份认证时使用密码。我们都在使用它们，并且它们不会很快到达任何地方。OK，下一步。我们还有什么可以并且应该用于基于用户和设备的身份验证和登录流程？

## 我们现在应该怎么办 - MFA(多因子认证)创可贴

<span style="padding-left:28px"></span> 所以我们承认用户名和密码的传统组合对我们的（系统）健康不利。接下来，多因子认证，还是双因子认证。你需要做出选择. 这通常会以令牌，电话作为令牌或某种其他带外机制的形式引入您将拥有的某些内容，从而创建一次性密码。传统上，“带外”机制是通过电子邮件或短信发送至预先注册的地址或电话号码，其中包含6位数的密码。内部系统或员工系统通常会使用硬标记 - 无论是USB加密狗还是带有小显示屏的密码生成器。从安全角度来看，这些概念当然更好，但是一方面可能会被破解，另一方面会经常创建不连贯的用户登录体验，其中有许多中断和用户交互。

<div style="text-align: center">
  <img src="https://1.bp.blogspot.com/-QvTh01aygE0/WRGFNcyuXQI/AAAAAAAAArg/nQMwevi-WBA-iCdP0QHK_vXtFP5Xs54cQCEw/s1600/2FA.png" />
</div>

## 我们接下来怎么办 - 十因子认证！

<span style="padding-left:28px"></span> 所以，用户名和密码的方式不是特别好。多因子认证很简单，实施起来相当便宜，但这意味着最终用户需要随身携带一些东西或者不断被要求输入一次性密码而中断登录流程。我们需要的不是双因子认证，而是十因子认证！更多的因子. 至少10个以上的因子。增加因子，旨在减少单一因子的风险，同时减少用户中断次数。这10个因子 (可能是 8, 也可能是 15, 你懂的) 都是为登录流程引入更广泛的因子分析。

<span style="padding-left:28px"></span> 每个因子都必须更加紧密和模块化，分析登录流程的一个部分。登录流程仍然可以利用很多的静态配置文件相关数据 (如用户名), 但增加了更多的上下文-位置, 时间, 设备起源的请求和比较因子, 查看以前的登录请求, 以确定模型或异常。

<div style="text-align: center">
  <img src="https://1.bp.blogspot.com/-fALv_iiByhs/WRGFi0aYCmI/AAAAAAAAArg/x8jbYPvvtcU2enIPicRJ7bxwxBbDHw5bQCEw/s400/dec-authentication.png" />
</div>

<span style="padding-left:28px"></span> 这些因子中的一些可能会 "通过", 一些可能会在登录过程中 "失败", 但这个过程很大程度上是关于积累和分析风险, 因此能够更准确地应对高风险。对每个用户登录应用双因子认证并不会降低风险，它只是对每个角色都检测全部的风险.

<span style="padding-left:28px"></span> 为经常变更机器，位置，网络和时区的诚挚用户优化登录体验不是更好吗？为用户提供更多的登录流程选择，并且在高风险情况发生时提供多种选择不是更好吗？

<span style="padding-left:28px"></span> 我认为认证正在走向的另一个关键领域是透明度。大家都在说关于"无摩擦", "轻松" 或 "零负担" 的登录。如果, 作为一个最终用户, 我报名, 牺牲隐私的设备指纹, 也许下载一个 OTP 或推消息的应用程序, 为什么我不能在不打断我的交互体验的情况下"登录"？经典的安全和便利的悖论。通过引入更多因子，并将这些因子与处理逻辑“粘合”在一起，用户认证系统可以更加灵敏 - 可能模仿状态机，以非确定性方式设计，其中任何给定因子可能具有多个结果。

## 我们希望达到的目的 - 透明的预先验证

<span style="padding-left:28px"></span> 所以我想科幻的最终目标就是在工作/咖啡店/门/汽车/网站/应用程序（适用时删除）这些场景中只出现一个真正的我。这项服务不仅会 "知道" 你是谁, 而且也相信你就是你。有点像女王，看到的时候不会怀疑。每次你展现自己时, 透明的后台检查都会不断地评估交互的每个部分, 寻找变化并确定风险。

<div style="text-align: center">
  <img src="https://4.bp.blogspot.com/-YAIfnGba1gw/WRGGCI6iyJI/AAAAAAAAAro/k-SQd4lZd8s99EhYsijTMHx58H3l1G2TQCEw/s320/Transparent_PreId.png" />
</div>

<span style="padding-left:28px"></span> 我们在网络世界中，离我们最近的的是cookie / session / tokenId / access_token的认证过程交换。无论是无状态的还是有状态的，当用户尝试再次访问服务时，这些令牌信息都会代表用户。将该令牌与某种绑定（PKI密钥对或TLS会话）相结合以减少令牌盗用的影响，并且在某些情况下可以重复访问。但是, 到处都是变化,因此，令牌的展示必须与令牌试图访问的所有用法、上下文、资源和事务数据相结合, 以允许身份验证机器循环通过必要的多重因子, 无论是单独或一起地确定风险或变化。

<span style="padding-left:28px"></span> 身份验证正在继续。在分析谁触发了一个事务的时候, 一个更加现代化的系统必须适应更广泛的场景, 同时还必须与提高透明度和预先确定风险的机制相结合, 而不需要不必要的和鲁莽的用户体验中断。

原文链接：https://forum.forgerock.com/2017/05/design-modern-authentication/