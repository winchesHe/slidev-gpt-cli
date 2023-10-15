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
---
# 提示词优化技巧

原则一 编写清晰、具体的指令

1.1 使用分隔符清晰地表示不同的输入部分

可以选择用 ````，"""，< >，<tag> </tag>，:` 等做分隔符，只要能明确起到隔断作用即可

```ts
const text = `
把用三个引号括起来的文本总结成一句话。
"""
${text}
"""
`
```

1.2 寻求结构化输出
有时候只想要某种格式内容，则需要指定对应的格式

1.3 要求模型检查是否满足条件

1.4 提供少量的示例（Few-shot）

>详细请查看 Open AI 给出的 6 种提问技巧

[https://platform.openai.com/docs/guides/gpt-best-practices](https://platform.openai.com/docs/guides/gpt-best-practices)

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
