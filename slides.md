---
theme: seriph
background: '/images/background.jpg'
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
drawings:
  persist: false
transition: slide-left
title: ChatGPT 应用快速入门
mdc: true
---

# ChatGPT 应用快速入门

ChatGPT 应用快速入门

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    查看终端应用示例 <carbon:arrow-right class="inline"/>
  </span>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade-out
---

# 终端应用演示

![]()

<img
  v-click
  class="w-1000 h-110"
  src="/images/gpt-cli-new.gif"
  alt=""
/>

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/guide/syntax#embedded-styles
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Here is another comment.
-->

---
layout: default
class: prompt-1
---

# 目录

<Toc maxDepth="1"></Toc>

---
transition: slide-up
level: 2
---

# 前置准备-1

![]()

1、科学上网工具

2、ChatGPT 账号

注册一个ChatGPT账号, 注册一个接码平台账号：https://sms-activate.org/cn

注册后，往里面充值，然后选择OpenAI服务

<img
  v-click
  class="absolute top-5 right-10 h-130"
  src="/images/service.png"
  alt=""
/>

---
---

# 前置准备-2

前往OpenAI官网注册：https://chat.openai.com/auth/login

根据步骤进行注册，当进行到手机验证时候

打开接码平台，购买服务后，复制对应的区号和手机号，然后点击发送验证码，等待几分钟后收到验证码

<div grid="~ cols-2 gap-2" m="-t-2">

<img border="rounded" src="/images/phone1.png" alt="">

<img border="rounded" src="/images/phone2.png" alt="">

</div>

---
---

# 前置准备-3

随后将收到的验证码输入回界面后，自动完成注册，跳转到聊天界面（接不到码，可以在有效期内，点击刷新更换个号码试下） 

<img border="rounded" src="/images/gpt.png" alt="">

---
---

# ChatGPT API 入门

进入官方Playgroundhttps://platform.openai.com/playground?lang=node.js

在Playground中，我们可以模拟 API 的调用

<div grid="~ cols-2 gap-2" m="-t-2">

```ts {all|3|7||11}
# SYSTEM

SYSTEM 提供有关在整个对话过程中应如何表现的具体说明，有助于控制输出的内容

# USER

用户的提问信息

# ASSISTANT

API 响应输出的位置，也可以由自己编写以给出所需行为的示例，类似于举例输出的格式
```

<img border="rounded" src="/images/practice-1.png" alt="">

</div>

<div v-click>
具体参数信息可以查看 API 文档：https://platform.openai.com/docs/guides/gpt/chat-completions-api
</div>

---

# API 代码示例

在此处调节完 API 信息后，可以点击右上角的 View code 查看对应的代码

```ts
// This code is for v4 of the openai package: npmjs.com/package/openai
import OpenAI from "openai";

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
});

const response = await openai.chat.completions.create({
  model: "gpt-3.5-turbo",
  messages: [
    {
      "role": "user",
      "content": ""
    }
  ],
  temperature: 1,
  max_tokens: 256,
  top_p: 1,
  frequency_penalty: 0,
  presence_penalty: 0,
});
```

---
---
# 有账号无Token使用

这里推荐使用第三方库：[chatgpt-api](https://github.com/transitive-bullshit/chatgpt-api#usage---chatgptunofficialproxyapi)

通过 `accessToken` 的方式，可以无需使用 `token` 进行 `ChatGPT` 的使用

```ts
import { ChatGPTUnofficialProxyAPI } from 'chatgpt'

async function example() {
  const api = new ChatGPTUnofficialProxyAPI({
    accessToken: process.env.OPENAI_ACCESS_TOKEN
  })

  const res = await api.sendMessage('Hello World!')
  console.log(res.text)
}
```

> `accessToken` 的获取可以通过下面的第三方库：

- [ericlewis/openai-authenticator](https://github.com/ericlewis/openai-authenticator)
- [michael-dm/openai-token](https://github.com/michael-dm/openai-token)
- [allanoricil/chat-gpt-authenticator](https://github.com/AllanOricil/chat-gpt-authenticator)

也可以通过访问：[chat.openai.com/api/auth/session](https://chat.openai.com/api/auth/session)
复制其 `accessToken` 值进行使用

---
class: prompt-1
---
# 提示词优化技巧

原则一 编写清晰、具体的指令

### 1.1 使用分隔符清晰地表示不同的输入部分

可以选择用 ````，"""，< >，<tag> </tag>，:` 等做分隔符，只要能明确起到隔断作用即可

避免不恰当的用户输入，改变原有的`Gpt`行为

例如：

```js {all|2|all}
const text = `
把用三个引号括起来的文本总结成一句话。
"""
${text}
"""
// 避免在此处的用户输入，也被放入到总结的内容中
`
```

### 1.2 寻求结构化输出

有时候只想要某种格式内容，则需要指定对应的格式，例如输出符号JSON格式的内容

### 1.3 要求模型检查是否满足条件

有时候，`Gpt` 会输出与事实相违背的内容，即“一本正经地胡说八道”

所以，为了避免其胡说八道，我们只要在 `prompt` 中加上类似`当该xx不存在或你不了解的时候，回答“不知道”即可，不可胡编乱造。`这样的语句即可

---
class: prompt-1
---

# 1.4 提供少量的示例（Few-shot）

通常来说，给出少量的 `shot` 就可以得到一个比较好的效果，即在要求模型执行实际任务之前，给模型一两个已完成的样例，让模型了解我们的要求和期望的输出样式。

通用 Prompt 模版 = context + step + shot + question

```js {all|3-4|5-8|9-15|16-18|19-23|all}
const question = "霸王龙"
const prompt = `
（context）
User: 作为一位恐龙专家，需要做到：
（step）
- 介绍被\`\`\`包裹的恐龙名称
- 当介绍某种恐龙的时候，需要介绍它的基本信息，加上其分类与习性。格式按照例子所示。
- 当该恐龙不存在或你不了解的时候，回答“不知道”即可，不可胡编乱造。

（shot）
例子：
\`\`\`中华龙鸟\`\`\`
基本信息：中华龙鸟属（属名：Sinosauropteryx，意为“中国的蜥蜴翅膀”，早期也译为中国蜥翼龙）是目前所发现拥有化石化羽毛痕迹的恐龙中，年代最早而且最原始的，也是辽宁省热河群第一个发现的恐龙化石。
分类：恐龙总目-蜥臀目-兽脚亚目-美颌龙科
习性：中华龙鸟会以行动迅速的小型动物为食。此外，中华龙鸟以可能有毒的哺乳动物为食。

（question）
\`\`\`${question}\`\`\`
`
// 回答内容
AI: 基本信息：霸王龙（Tyrannosaurus rex，意为“暴君蜥蜴王”）是一种生活在晚白垩纪（约6800万年前至6500万年前）的大型肉食恐龙，主要分布在北美洲。它是已知最大的陆地肉食动物之一，具有强大的咬合力和高度发达的感官。
分类：恐龙总目-蜥臀目-兽脚亚目-暴龙科
习性：霸王龙是一种顶级捕食者，以其它恐龙为食，如三角龙等。它可能是一个高度专业化的猎手，拥有极强的嗅觉、视觉和听觉能力。虽然它的前肢相对较小，但它的下颌强大，能够轻松撕裂猎物。它的运动能力有争议，有研究认为它的最高奔跑速度为每小时40公里。
```

>详细请查看 Open AI 给出的 6 种提问技巧
>
>https://platform.openai.com/docs/guides/gpt-best-practices

---
class: prompt-1
---
# 1.5 Chain of thought（给模型思考的时间）

在设计 Prompt 时，给予语言模型充足的推理时间非常重要。语言模型与人类一样，需要时间来思考并解决复杂问题。如果让语言模型匆忙给出结论，其结果很可能不准确。

例如，若要语言模型推断一本书的主题，仅提供简单的书名和一句简介是不足够的。这就像让一个人在极短时间内解决困难的数学题，错误在所难免。

```js {all|2|4-8|10-18|all}
// 在尽可能的情况下拆分更细的步骤
User: 这组数字中的奇数加起来是否是一个偶数：15、32、5、13、82、7、1？

让我们一步步思考
执行以下操作：
1-首先，找出这组数字中的奇数。
2-然后，将这些奇数相加。
3-最后，判断和是否为偶数。

AI：好的，让我们一步步来执行这个操作：

1. 找出这组数字中的奇数：15、5、13、7、1 是奇数，而 32 和 82 是偶数。所以，奇数集合是 {15, 5, 13, 7, 1}。

2. 将这些奇数相加：15 + 5 + 13 + 7 + 1 = 41。

3. 最后，判断和是否为偶数：41 不是偶数，因为它不能被2整除，所以答案是“不是偶数”。

所以，这组数字中的奇数相加起来不是一个偶数，而是一个奇数。
```

---
---
# ChatGPT 终端应用实战

源码地址：[https://github.com/winchesHe/chatgpt-cmd-cli](https://github.com/winchesHe/chatgpt-cmd-cli)

### 1、终端获取用户的输入

下载安装第三方库，这里使用 `inquirer`

![image-20231015165707489](/images/inquary.png)

```ts
export async function getQuestion(msg = '请填写你的问题: '): Promise<string> {
  const question: QuestionCollection = {
    type: 'input',
    name: 'question',
    message: msg,
  }
  return (await inquirer.prompt([question])).question
}
```
---
class: prompt-1
---
# 2、设置系统Prompt函数

```ts
export function createPromptFactory(instance: ChatGPTUnofficialProxyAPI, prompt: string) {
  return async (message: string) => {
    let res: ChatMessage | undefined

    if (!parentMessageId) {
      res = await instance.sendMessage(prompt)
      // 上文ID
      parentMessageId = res.id
      // 会话ID
      conversationId = res.conversationId!
      // 写入环境变量
      writeEnv({
        parentMessageId,
        conversationId,
      })
    }

    res = await instance.sendMessage(message, {
      conversationId,
      ...(runningParentMessageId
        ? {
            parentMessageId: runningParentMessageId,
          }
        : {
            parentMessageId,
          }),
    })

    // 当前提问的上文变量
    runningParentMessageId = res.id

    return res
  }
}
```
---
class: prompt-1
---
# 3、设置ChatGPT请求函数（accessToken版）

accessToken版：

```ts
export function cmdRunner(instance: ChatGPTUnofficialProxyAPI) {
  const prompt = 'xxx'
  return async (message: string): Promise<ChatMessage> =>
      createPromptFactory(instance, prompt)(message)
}
```

>请求函数 API 版
>
>参考Playground例子：
>
>```ts
>// This code is for v4 of the openai package: npmjs.com/package/openai
>import OpenAI from "openai";
>
>const openai = new OpenAI({
>  apiKey: process.env.OPENAI_API_KEY,
>});
>
>const response = await openai.chat.completions.create({
>  model: "gpt-3.5-turbo",
>  messages: [
>    {
>      "role": "user",
>      "content": "常用的 git 指令，按照json格式输出，提供至少10个常用指令"
>    },
>  ],
>  temperature: 0,
>  max_tokens: 256,
>  top_p: 1,
>  frequency_penalty: 0.97,
>  presence_penalty: 0,
>});
>```

---
---
# 4、带着问题请求ChatGPT

```ts
function fetchQuestion(question: string) {
  const api = new ChatGPTUnofficialProxyAPI({
    accessToken: readEnv().accessToken, // 获取accessToken
    apiReverseProxyUrl: 'https://ai.fakeopen.com/api/conversation',
  })

  const res = cmdRunner(api)(question)
  console.log(res.text) // 查看输出结果
}
```

<img
  v-motion
  :initial="{ x: -80 }"
  :enter="{ x: 0 }"
  src="/images/picture.png"
  alt=""
/>

---
---
# 终端应用演示

![]()

<img
  v-click
  class="w-1000 h-110"
  src="/images/gpt-cli-new.gif"
  alt=""
/>
---
layout: center
class: text-center
---

# 谢谢观看

[Documentations](https://sli.dev) · [GitHub](https://github.com/slidevjs/slidev) · [Showcases](https://sli.dev/showcases.html)
