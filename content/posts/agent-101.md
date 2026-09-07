---
title: "AI Agent 入门：从 LLM 到能办事的助手"
date: 2026-09-06
posttype: "教程"
homeblock: tutorial
summary: "什么是 Agent、为什么现在火、自己怎么搭一个——写给程序员的入门笔记。"
draft: false
slug: "agent-101"
description: "一篇写给程序员的 Agent 入门笔记：什么是 Agent、为什么现在火、自己怎么搭一个"
tags: ["Agent", "LLM", "入门"]
categories: ["AI与工具"]
related: ['tech-tools']
---

## 什么是 Agent

如果 LLM 是个**很会说话的大脑**，那 Agent 就是给它装上**手脚**。
大脑只负责「想」，手脚负责「做」—— 调 API、读写文件、操作浏览器、写代码。

一个最小可用的 Agent 通常有三件套：

1. **LLM**：负责推理和决策
2. **工具（Tools）**：Agent 能调用的函数，每个工具是一段「副作用」
3. **循环（Loop）**：让 Agent 反复「观察 → 思考 → 行动」直到任务完成

```python
while not done:
    thought = llm(messages, tools)
    if thought.tool_call:
        result = run(thought.tool_call)
        messages.append(result)
    else:
        return thought.final_answer
```

这就是 ReAct 框架的雏形。

## 为什么现在火

三个变量同时凑齐了：

- **模型够强**：o1 / Claude 4 / DeepSeek-R1 这类模型在多步推理上已经稳定
- **协议成熟**：MCP、Function Calling、Tool Use 的接口基本统一
- **生态到位**：LangChain、LlamaIndex、CrewAI、AutoGen 一堆框架随便挑

## 自己搭一个最小 Agent

不依赖任何框架，纯 Python 几十行就能跑：

```python
import json, subprocess
from openai import OpenAI

client = OpenAI()

TOOLS = [{
    "type": "function",
    "function": {
        "name": "run_cmd",
        "description": "在本地执行 shell 命令并返回输出",
        "parameters": {
            "type": "object",
            "properties": {"cmd": {"type": "string"}},
            "required": ["cmd"]
        }
    }
}]

def run(cmd: str) -> str:
    return subprocess.run(cmd, shell=True, capture_output=True, text=True).stdout

messages = [{"role": "user", "content": "列出当前目录所有 .py 文件"}]
for _ in range(5):
    r = client.chat.completions.create(model="gpt-4o-mini", messages=messages, tools=TOOLS)
    msg = r.choices[0].message
    messages.append(msg)
    if msg.tool_calls:
        for tc in msg.tool_calls:
            out = run_cmd(json.loads(tc.function.arguments)["cmd"])
            messages.append({"role": "tool", "tool_call_id": tc.id, "content": out})
    else:
        print(msg.content); break
```

**重点不是代码本身**，而是循环结构——
只要让模型「能看结果 → 修正下一步」，它就能干很多事。

## 常见误区

- **把 Agent 当万能解药**：90% 的任务单次 LLM 调用就够了，套上 Agent 反而慢且不稳
- **工具太多**：工具一多模型选择困难，幻觉率飙升。**单 Agent ≤ 5 个工具** 是经验法则
- **没设上限**：循环一定要有 max_iterations，否则模型可能死循环烧光 token
- **不记日志**：Agent 出错时没 trace 排不出来，所有调用必须落盘

## 下一步

下一篇我会拆一个**真实可用的 Coding Agent**：
基于 Claude Code 的工作流，从需求到 PR 全程如何编排。

如果你也在做 Agent，欢迎交流 👋