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


# AI Model Comparison Matrix

## Best vs Cost Savings

| Category | Best | Cost Savings |
|----------|------|--------------|
| **BRAIN** | Opus 4.5 | Kimi K2.5 |
| **HEARTBEAT** | Haiku | Haiku |
| **CODING** | Codex GPT 5.2 xHigh | MiniMax 2.1 |
| **WEB SEARCH** | Opus 4.5 | Deepseek v3 |
| **CONTENT** | Opus 4.5 | Kimi K2.5 |
| **VOICE** | ChatGPT 4o Realtime | ChatGPT 4o Realtime |
| **IMAGE UNDERSTANDING** | Opus 4.5 | Gemini 2.5 Flash |

---

## Analysis

This matrix presents a strategic breakdown of AI model selection across seven key use cases, comparing "best-in-class" options against "cost-effective" alternatives.

**Key Insights:**

- **Opus 4.5 dominates premium tier**: Claude's Opus 4.5 appears as the "Best" choice for Brain, Web Search, Content, and Image Understanding tasks
- **Haiku as efficiency winner**: Claude's Haiku model is optimal for both best and cost-effective "Heartbeat" (presumably high-frequency/routine) operations
- **OpenAI monopoly on voice**: ChatGPT 4o Realtime holds both best and cost-effective positions for voice interaction
- **Emerging alternatives**: Chinese models (Kimi K2.5, Deepseek v3, MiniMax 2.1) positioned as cost-effective substitutes
- **Google's niche**: Gemini 2.5 Flash serves as budget option for image understanding only

**Strategic Implications:**

This appears to be an operational decision matrix for a production system that needs to balance performance with cost. The framework suggests a hybrid deployment strategy where mission-critical cognitive tasks justify premium Opus 4.5 pricing, while routine operations and specific workloads can leverage cheaper alternatives without significant quality degradation.

**The absence of Sonnet 4.5 is notable—suggesting this analysis predates its release or considers it a middle-ground option**
