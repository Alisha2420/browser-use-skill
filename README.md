# 🌐 Browser-Use Skill for OpenClaw

> **Stop fighting with snapshot→act loops.** Let AI handle complex browser automation end-to-end.

[![ClawHub](https://img.shields.io/badge/ClawHub-browser--use-blue?style=flat-square)](https://clawhub.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)
[![Browser-Use](https://img.shields.io/badge/Powered_by-Browser--Use-orange?style=flat-square)](https://github.com/browser-use/browser-use)
[![Wiki](https://img.shields.io/badge/📖_Wiki-7_pages-purple?style=flat-square)](https://github.com/abczsl520/browser-use-skill/wiki)

---

## 😤 The Problem

OpenClaw's built-in browser tool is great for simple tasks — screenshot, click a button, done. But for **multi-step workflows**, it becomes a nightmare:

```
Agent: *takes snapshot* → *clicks wrong button* → *takes snapshot* → *page changed* → 
       *confused* → *clicks again* → *popup appeared* → *lost* → ❌ Failed
```

Sound familiar? Login forms, dynamic pages, popups, anti-bot detection — the built-in tool wasn't designed for these.

## ✅ The Solution

This skill integrates [**Browser-Use**](https://github.com/browser-use/browser-use) (38k+ ⭐) — an AI browser agent that **sees pages like a human** and completes entire workflows autonomously.

```
You:          "Log into Reddit and post this article to r/python"
Browser-Use:  ✅ Opens login → types credentials → handles CAPTCHA wait → 
              navigates to submit → fills title & body → clicks Post → returns URL
```

**One task in, result out.** No manual step-by-step babysitting.

## ⚡ Quick Start

```bash
# 1. Install the skill
clawhub install browser-use

# 2. Setup Python environment (one-time)
python3 -m venv ~/browser-use-env
source ~/browser-use-env/bin/activate
pip install browser-use playwright langchain-openai
playwright install chromium
```

Then just tell your OpenClaw agent:

> *"用 browser-use 登录 Reddit 发个帖子"*

The skill handles everything: mode selection, script generation, execution, and error recovery.

## 🤔 When to Use What

| Scenario | Built-in `browser` | This Skill |
|----------|:---:|:---:|
| Take a screenshot | ✅ Free & instant | ❌ Overkill |
| Click one button | ✅ | ❌ |
| **5+ step workflow** (login→navigate→fill→submit) | ❌ Breaks easily | **✅ Autonomous** |
| **Anti-bot sites** (Reddit, LinkedIn, Twitter) | ❌ Detected | **✅ Real Chrome** |
| **Batch operations** | ❌ | **✅** |
| **Data scraping** with complex navigation | ❌ Manual | **✅ Smart** |

> **Rule of thumb:** If it takes more than 3 clicks, use Browser-Use.

## 🔑 Key Features

### 🎯 Smart Task Routing
The skill knows when Browser-Use is needed vs when the built-in tool is enough. No wasted API calls.

### 🔐 Secure Credential Handling
Passwords use placeholder substitution — the LLM **never sees your real credentials**:
```python
agent = Agent(
    task="Login with x_user and x_pass",
    sensitive_data={"x_user": "real@email.com", "x_pass": "S3cret!"},
)
```

### 🛡️ Anti-Detection (Real Chrome Mode)
Connect to your actual Chrome browser via CDP — sites see a real human, zero detection:
```python
browser = Browser(cdp_url="http://127.0.0.1:9222")
```

### ⚡ Flash Mode
Skip LLM reasoning for simple steps — 2x faster:
```python
agent = Agent(task="...", flash_mode=True)
```

### 🔧 Built-in Failure Recovery
CAPTCHA? Timeout? Anti-spam? The skill includes a complete decision tree for common failures.

## 📖 Quick Example

```python
import asyncio
from browser_use import Agent, ChatOpenAI, Browser

async def main():
    llm = ChatOpenAI(model="gpt-4o-mini", api_key="YOUR_KEY")
    browser = Browser(cdp_url="http://127.0.0.1:9222")  # Real Chrome
    
    agent = Agent(
        task="""
        1. Go to https://news.ycombinator.com
        2. Extract the top 5 story titles and URLs
        3. Return them as a formatted list
        """,
        llm=llm, browser=browser, use_vision=True, max_steps=15,
    )
    result = await agent.run()
    print(result)

asyncio.run(main())
```

## 🎮 LLM Compatibility

| LLM | Works | Best For |
|-----|:---:|---------|
| GPT-4o-mini | ✅ | **Default choice** — fast & cheap |
| GPT-4o | ✅ | Complex reasoning tasks |
| Claude 3.5+ | ✅ | Good alternative |
| Gemini | ❌ | Structured output incompatible |

## 📚 Documentation

📖 **[Full Wiki →](https://github.com/abczsl520/browser-use-skill/wiki)**

| Guide | What You'll Learn |
|-------|-------------------|
| [Getting Started](https://github.com/abczsl520/browser-use-skill/wiki/Getting-Started) | Install, setup, first automation |
| [Mode A vs Mode B](https://github.com/abczsl520/browser-use-skill/wiki/Mode-A-vs-Mode-B) | Built-in Chromium vs Real Chrome |
| [Task Writing Guide](https://github.com/abczsl520/browser-use-skill/wiki/Task-Writing-Guide) | Write prompts that work first try |
| [Sensitive Data](https://github.com/abczsl520/browser-use-skill/wiki/Sensitive-Data) | Secure password handling + 2FA |
| [Real-World Examples](https://github.com/abczsl520/browser-use-skill/wiki/Real-World-Examples) | Copy-paste recipes |
| [Troubleshooting](https://github.com/abczsl520/browser-use-skill/wiki/Troubleshooting) | Fix common issues |
| [FAQ](https://github.com/abczsl520/browser-use-skill/wiki/FAQ) | Quick answers |

## 🔗 Related

- [Browser-Use](https://github.com/browser-use/browser-use) — The underlying browser AI framework (38k+ ⭐)
- [OpenClaw](https://github.com/openclaw/openclaw) — The AI agent platform this skill runs on
- [Bug Audit](https://github.com/abczsl520/bug-audit-skill) — Dynamic bug hunting (200+ patterns)
- [Debug Methodology](https://github.com/abczsl520/debug-methodology) — Root-cause debugging for AI agents
- [Game Quality Gates](https://github.com/abczsl520/game-quality-gates) — 12 universal game dev quality checks

## 🤝 Contributing

Found a bug? Have an idea? [Open an issue](https://github.com/abczsl520/browser-use-skill/issues) or submit a PR!

## 📄 License

[MIT](LICENSE) — Use it however you want.

---

<p align="center">
  <b>⭐ If this skill saved you time, consider starring the repo — it helps others find it!</b>
</p>
