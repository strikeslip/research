## 🦀 openclaw performance optimization playbook:

- add deepseek ($0.14/$1.10) for routine ops vs sonnet ($3/$15) for complex work
- set contextTokens to 120k not 150k, prevents the dreaded freeze when context overflows 200k limit
- build auto-healing watchdog scripts that restart services when they hang
- enable cross-context messaging so your telegram agent can post to slack channels
- backup your config daily, restarts wipe everything without it
- model hierarchy opus for reasoning, sonnet for moderate tasks, deepseek for everything else

or (if i could shill my own project) you can just wait on the next version of gru which automates all of this for you

ref: https://x.com/0xzak/status/2018871985254617295<br>

- try Grok 4.1
- howto implement/use https://venice.ai/venice-api (use Kimi K2.5)
- Heurist Mesh Crypto Analysis Skill https://www.clawhub.ai/wjw12/heurist-mesh
- DeepWiki https://deepwiki.com/openclaw/openclaw
- OpenClaw Skills https://github.com/VoltAgent/awesome-openclaw-skills
- Moltbook https://github.com/moltbook
- clawstr https://github.com/clawstr/clawstr

<br>
<br>

| Category | Best | Cost Savings |
|----------|------|--------------|
| **BRAIN** | Opus 4.5 | Kimi K2.5 |
| **HEARTBEAT** | Haiku | Haiku |
| **CODING** | Codex GPT 5.2 xHigh | MiniMax 2.1 |
| **WEB SEARCH** | Opus 4.5 | Deepseek v3 |
| **CONTENT** | Opus 4.5 | Kimi K2.5 |
| **VOICE** | ChatGPT 4o Realtime | ChatGPT 4o Realtime |
| **IMAGE UNDERSTANDING** | Opus 4.5 | Gemini 2.5 Flash |

<br>
- Sonnet 4.5 should be used also..

