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
- howto implement/use https://venice.ai/venice-api
- Heurist Mesh Crypto Analysis Skill https://www.clawhub.ai/wjw12/heurist-mesh
