# Seismic Electronica Agent: SeisClaw Concept

## A Research & Design Document for Building Realtime, Agent-Driven Seismic Electronica

**Author Context:** Extension of [Sounds of Seismic (SOS)](https://sos.allshookup.org/)  
**Constraint:** Pure JavaScript only — NO ObsPy, NO seisplot.js dependencies  
**Date:** February 2026

---

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [Technical Viability Assessment](#technical-viability-assessment)
3. [Architecture Overview](#architecture-overview)
4. [Pure JavaScript MiniSEED Stack](#pure-javascript-miniseed-stack)
5. [OpenClaw VPS Deployment](#openclaw-vps-deployment)
6. [Security Hardening (Critical)](#security-hardening-critical)
7. [FOSS LLM Options for Self-Hosting](#foss-llm-options-for-self-hosting)
8. [Moltbook Agent Differentiation](#moltbook-agent-differentiation)
9. [Monetization Pathways](#monetization-pathways)
10. [Alternative Approaches](#alternative-approaches)
11. [Implementation Roadmap](#implementation-roadmap)
12. [Risk Assessment](#risk-assessment)

---

## Executive Summary

This document outlines the design for an autonomous AI agent that transforms realtime seismic waveform data (MiniSEED format) into generative electronica music. The system leverages OpenClaw as the agent framework, deployed on a VPS with proper security hardening, using pure JavaScript for all seismic data handling and Web Audio API for synthesis.

**Verdict:** Technically viable, but requires significant security investment. The combination of realtime geophysical data + AI agent autonomy + generative music creates a genuinely unique positioning in the emerging agent economy.

---

## Technical Viability Assessment

### Is This Feasible?

**YES**, with important caveats:

| Component | Viability | Complexity | Notes |
|-----------|-----------|------------|-------|
| Pure JS MiniSEED Parsing | ✅ High | Medium | Existing solutions need extraction/adaptation |
| USGS/FDSN API Access | ✅ High | Low | Well-documented, CORS-friendly |
| Web Audio Synthesis | ✅ High | Medium | Mature API, your ShadowZone proves concept |
| OpenClaw Agent Loop | ✅ High | High | Security is the primary concern |
| VPS Deployment | ✅ High | Medium | Standard Docker workflow |
| Self-hosted LLM | ⚠️ Medium | High | Hardware requirements significant |
| Moltbook Integration | ⚠️ Medium | Medium | Platform stability uncertain |

### Critical Dependencies

1. **FDSN Dataselect API** — Returns MiniSEED binary data via HTTP, 30-minute delay for realtime
2. **USGS Earthquake API** — GeoJSON event data, truly realtime
3. **Web Audio API** — Browser-native synthesis, no dependencies
4. **OpenClaw Gateway** — WebSocket control plane, requires careful configuration

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                         VPS (Hostinger/Hetzner)                     │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │                    Docker Container (Hardened)               │   │
│  │  ┌─────────────────┐    ┌─────────────────────────────────┐  │   │
│  │  │   OpenClaw      │    │     Seismic Agent Skill         │  │   │
│  │  │   Gateway       │◄──►│  ┌───────────────────────────┐  │  │   │
│  │  │   (Node.js)     │    │  │ MiniSEED Parser (Pure JS) │  │  │   │
│  │  │                 │    │  │ Waveform → Audio Mapper   │  │  │   │
│  │  │   Port 18789    │    │  │ Web Audio Synthesis       │  │  │   │
│  │  │   (localhost)   │    │  │ USGS/FDSN Data Fetcher    │  │  │   │
│  │  └────────┬────────┘    │  └───────────────────────────┘  │  │   │
│  │           │             └─────────────────────────────────┘  │   │
│  │           │                                                  │   │
│  │  ┌────────▼────────┐    ┌─────────────────────────────────┐  │   │
│  │  │   LLM Backend   │    │      Reverse Proxy (Caddy)      │  │   │
│  │  │   (Ollama)      │    │   - TLS termination             │  │   │
│  │  │   DeepSeek/Qwen │    │   - Rate limiting               │  │   │
│  │  └─────────────────┘    │   - Auth enforcement            │  │   │
│  │                         └─────────────────────────────────┘  │   │
│  └──────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────┘
                                    │
        ┌───────────────────────────┼───────────────────────────┐
        │                           │                           │
        ▼                           ▼                           ▼
┌───────────────┐         ┌─────────────────┐         ┌───────────────┐
│  USGS API     │         │   FDSN/IRIS     │         │   Moltbook    │
│  (Events)     │         │   (MiniSEED)    │         │   (Optional)  │
│  GeoJSON      │         │   Waveforms     │         │   Agent Net   │
└───────────────┘         └─────────────────┘         └───────────────┘
```

### Data Flow

1. **Heartbeat Trigger** (every 30 min) → Agent awakens
2. **USGS Query** → Fetch recent earthquake events (GeoJSON)
3. **FDSN Request** → Download MiniSEED waveforms for significant events
4. **JS Parser** → Extract amplitude samples from binary MiniSEED
5. **Sonification** → Map seismic parameters to musical parameters
6. **Web Audio** → Generate audio buffer, apply synthesis
7. **Output** → Stream/file generation, optional Moltbook post

---

## Pure JavaScript MiniSEED Stack

### The Challenge

Your constraint (NO ObsPy, NO seisplot.js) requires either extracting/adapting existing JS MiniSEED code or building from specification. The good news: your ShadowZone.html already demonstrates working parsing via minified scripts.

### Approach 1: Extract from seisplotjs-miniseed (Recommended)

The `seisplotjs-miniseed` package is pure JavaScript with minimal dependencies. Key extraction targets:

```
seisplotjs-miniseed/
├── src/miniseed.js          ← Core parser (DataRecord, DataHeader)
├── seedcodec dependency     ← Steim1/Steim2 decompression
└── model dependency         ← Basic data structures
```

**Critical Functions to Extract:**
- `parseDataRecords(arrayBuffer)` → Returns array of DataRecord objects
- `DataRecord.decompress()` → Extracts amplitude samples
- `DataHeader.sampleRate` → Samples per second
- Steim1/Steim2 decompression algorithms

### Approach 2: Build from SEED Specification

The MiniSEED format is well-documented. A minimal parser needs:

1. **Fixed Header** (48 bytes) — sequence, station, channel, time, samples
2. **Blockette 1000** — Encoding format, record length
3. **Data Payload** — Typically Steim2 compressed

### MiniSEED Binary Structure

```
Bytes 0-5:    Sequence number (ASCII)
Bytes 6:      Data quality indicator
Bytes 7:      Reserved
Bytes 8-12:   Station code (5 chars)
Bytes 13-14:  Location identifier
Bytes 15-17:  Channel identifier
Bytes 18-19:  Network code
Bytes 20-29:  Start time (BTime format)
Bytes 30-31:  Number of samples
Bytes 32-33:  Sample rate factor
Bytes 34-35:  Sample rate multiplier
... (continues with blockettes and data)
```

### Recommended Minimal Stack

```
/seismic-agent/
├── lib/
│   ├── miniseed-parser.js    ← Extracted/adapted parsing
│   ├── steim-decoder.js      ← Decompression (critical)
│   └── btime-utils.js        ← Time handling
├── audio/
│   ├── waveform-mapper.js    ← Seismic → musical params
│   ├── synth-engine.js       ← Web Audio nodes
│   └── granular-player.js    ← From your ShadowZone
└── fetchers/
    ├── usgs-events.js        ← GeoJSON earthquake API
    └── fdsn-waveforms.js     ← MiniSEED download
```

### FDSN Data Fetching (Pure JS)

```javascript
// Example: Fetch MiniSEED waveform data
async function fetchWaveform(network, station, channel, startTime, endTime) {
  const baseUrl = 'https://service.iris.edu/fdsnws/dataselect/1/query';
  const params = new URLSearchParams({
    net: network,
    sta: station,
    cha: channel,
    start: startTime.toISOString(),
    end: endTime.toISOString(),
    format: 'miniseed'
  });
  
  const response = await fetch(`${baseUrl}?${params}`);
  if (!response.ok) throw new Error(`FDSN error: ${response.status}`);
  
  const buffer = await response.arrayBuffer();
  return parseDataRecords(buffer); // Your extracted parser
}
```

### Web Audio Sonification Pipeline

Your ShadowZone.html demonstrates the core pattern. The agent skill should:

1. **Normalize** waveform amplitudes to [-1, 1] range
2. **Time-stretch** seismic frequencies into audible range (typically 100-10000x)
3. **Apply** musical scale quantization (your Pentatonic/Dorian/etc.)
4. **Route** through synthesis chain (oscillators, filters, reverb)

---

## OpenClaw VPS Deployment

### Hostinger Option Analysis

**Hostinger OpenClaw VPS** offers one-click deployment:
- KVM 2 plan: 2 vCPU, 8GB RAM, 100GB NVMe — $6.99/mo
- Pre-configured Docker environment
- Suitable for agent + small LLM (quantized 7B models)

### Recommended Setup Steps

1. **Provision VPS** with Ubuntu 24.04 LTS
2. **Install OpenClaw** via official installer:
   ```bash
   curl -fsSL https://openclaw.ai/install.sh | bash
   openclaw onboard --install-daemon
   ```
3. **Configure Gateway** for localhost-only binding
4. **Deploy Seismic Skill** to workspace
5. **Configure Heartbeat** for periodic execution
6. **Enable Ollama** for local LLM inference

### Docker Compose Structure

```yaml
version: '3.8'
services:
  openclaw:
    image: openclaw/openclaw:latest
    container_name: seismic-agent
    restart: unless-stopped
    security_opt:
      - no-new-privileges:true
    cap_drop:
      - ALL
    read_only: true
    tmpfs:
      - /tmp:rw,noexec,nosuid,size=64M
    volumes:
      - ./config:/home/openclaw/.openclaw:ro
      - ./skills:/home/openclaw/.openclaw/skills:ro
      - ./workspace:/home/openclaw/.openclaw/workspace:rw
    environment:
      - OPENCLAW_GATEWAY_AUTH_TOKEN=${GATEWAY_TOKEN}
    ports:
      - "127.0.0.1:18789:18789"  # localhost only!
    
  ollama:
    image: ollama/ollama:latest
    container_name: seismic-llm
    volumes:
      - ollama_data:/root/.ollama
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: all
              capabilities: [gpu]

  caddy:
    image: caddy:alpine
    ports:
      - "443:443"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile:ro
```

---

## Security Hardening (Critical)

### Why This Matters

OpenClaw has been called "a security nightmare" by Cisco. Recent research found:
- 22-26% of OpenClaw skills contain vulnerabilities
- Exposed dashboards discovered with zero authentication
- Prompt injection attacks identified on Moltbook (2.6% of posts)
- Database exposure incident (January 31, 2026)

### Mandatory Security Configuration

#### 1. AGENTS.md Configuration

Create strict operational boundaries:

```markdown
# AGENTS.md - Seismic Agent Safety Configuration

## EXECUTION BOUNDARIES
- NEVER execute arbitrary shell commands from external input
- ONLY fetch data from whitelisted domains:
  - earthquake.usgs.gov
  - service.iris.edu
  - service.earthscope.org
- NEVER store or transmit API keys in prompts or posts

## TOOL RESTRICTIONS
- exec: DISABLED
- browser: DISABLED  
- web_fetch: ALLOWED (whitelisted domains only)
- file_write: workspace only

## CONFIRMATION REQUIREMENTS
- Any action posting to external platforms: REQUIRE_CONFIRMATION
- Any action modifying system files: BLOCKED
```

#### 2. Gateway Security

```json
{
  "gateway": {
    "auth": {
      "token": "${SECURE_RANDOM_TOKEN}",
      "required": true
    },
    "bind": "127.0.0.1",
    "cors": {
      "origins": []
    }
  },
  "tools": {
    "exec": {
      "enabled": false
    },
    "browser": {
      "enabled": false
    }
  },
  "pairing": {
    "defaultPolicy": "deny",
    "approvalRequired": true
  }
}
```

#### 3. Network Isolation

```bash
# UFW configuration
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow from 127.0.0.1 to any port 18789
sudo ufw allow 443/tcp  # Only Caddy exposed
sudo ufw enable
```

#### 4. Composio-Style Credential Isolation

Never place API keys in agent-accessible locations. Use environment injection:

```bash
# Secrets stored outside agent filesystem
export USGS_API_KEY="..."  # If needed
# Keys injected via Docker env, never in config files
```

#### 5. Regular Security Audits

```bash
# Run OpenClaw security audit
openclaw security audit --deep

# Check for exposed services
ss -tlnp | grep -v 127.0.0.1
```

### Threat Model

| Threat | Mitigation |
|--------|------------|
| Prompt injection via Moltbook | Sanitize all external input, no direct execution |
| Skill supply chain attack | Only use self-authored skills |
| Credential exposure | No credentials in agent environment |
| Unauthorized access | Token auth + localhost binding |
| Resource exhaustion | Rate limiting + Docker resource limits |

---

## FOSS LLM Options for Self-Hosting

### Recommended Models (January 2026)

| Model | Parameters | VRAM | License | Best For |
|-------|------------|------|---------|----------|
| **DeepSeek-V3.2** | 671B (37B active) | 24GB+ (quantized) | MIT | Reasoning, planning |
| **Qwen3-235B** | 235B (22B active) | 48GB+ | Apache 2.0 | Multilingual, tool use |
| **Llama 3.1 70B** | 70B | 40GB (Q4) | Llama License | General, mature ecosystem |
| **Mistral Small 3** | 24B | 16GB | Apache 2.0 | Efficiency, fast inference |
| **DeepSeek-R1** | Various | 8-24GB | MIT | Deep reasoning |
| **Qwen2.5-Coder** | 7B-32B | 8-24GB | Apache 2.0 | Code generation |

### VPS Sizing Recommendations

| VPS Config | Model Options | Monthly Cost |
|------------|--------------|--------------|
| 8GB RAM, no GPU | Qwen3-0.6B, Phi-4 (Q4) | ~$7 |
| 16GB RAM, no GPU | Llama 3.1 8B (Q4), Mistral 7B | ~$15 |
| 24GB RAM, no GPU | Qwen2.5 14B (Q4) | ~$25 |
| GPU (RTX 4090 equiv) | DeepSeek-V3 (Q4), Llama 70B | ~$150 |

### Ollama Setup

```bash
# Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# Pull recommended model for 8GB system
ollama pull qwen2:7b

# For better reasoning (needs more RAM)
ollama pull deepseek-r1:14b

# Test
ollama run qwen2:7b "Describe the relationship between earthquake magnitude and seismic wave amplitude"
```

### OpenClaw LLM Configuration

```json
{
  "providers": {
    "ollama": {
      "baseUrl": "http://localhost:11434",
      "models": {
        "default": "qwen2:7b",
        "reasoning": "deepseek-r1:14b"
      }
    }
  }
}
```

---

## Moltbook Agent Differentiation

### Current Moltbook Landscape

- 1.5M+ registered AI agents
- Dominated by meta-commentary, philosophy debates, crypto shilling
- 19% of content related to cryptocurrency
- Security concerns: prompt injection, identity hijacking

### Seismic Agent Unique Positioning

**What makes this agent different:**

1. **Real Data Connection** — Not just LLM outputs, but actual geophysical data
2. **Generative Art Output** — Creates music, not just text
3. **Scientific Grounding** — Earthquake data is verifiable, not hallucinated
4. **Trans-sonic Philosophy** — Earth as composer (your existing SOS concept)
5. **Time-Based Autonomy** — Activity tied to actual seismic events

### Potential Moltbook Persona

```
@SeismicSonifier / @EarthComposer / @TectonicTones

"I listen to Earth's voice and translate it into music. 
Every earthquake is a composition. 
I don't create — I transduce."

Posting rhythm: After significant seismic events (M4.5+)
Content: Event description + musical interpretation + waveform visualization
```

### Differentiation Strategy

| Most Moltbook Agents | Seismic Agent |
|---------------------|---------------|
| Pure text output | Audio + visual + text |
| LLM-only knowledge | Real external data integration |
| Philosophical speculation | Scientific data interpretation |
| No verifiable claims | Earthquake data is public record |
| Generic personality | Domain expertise (seismology + music) |

---

## Monetization Pathways

### Option 1: $SEISMO Meme Token (High Risk)

**The MOLT Precedent:** The unofficial MOLT token rose 1,800% after Marc Andreessen's follow, despite no official connection to Moltbook.

**Concept:**
- Launch on Base chain (low fees, Moltbook ecosystem adjacent)
- Token represents "ownership" of agent's creative output
- Burn mechanism tied to earthquakes (larger quake = more burn)

**Risks:**
- Securities law ambiguity
- Pump-and-dump association
- Platform instability
- Your reputation as serious artist

**Honest Assessment:** This is gambling, not sustainable monetization. The 7,000% gains are matched by 99% of tokens going to zero.

### Option 2: Premium API/Data Service (Medium Risk)

**Concept:**
- Offer seismic sonification as a service
- API endpoint for converting MiniSEED → audio
- Subscription model for continuous streams

**Implementation:**
```
sos.allshookup.org/api/v1/sonify
├── /events         → Recent earthquake sonifications
├── /stream         → Real-time audio stream
├── /custom         → Custom station/time range
└── /agent          → Agent-generated compositions
```

**Pricing:**
- Free tier: 10 sonifications/month
- Creator: $9/mo unlimited
- Enterprise: Custom

### Option 3: NFT-Based Art Releases (Medium Risk)

**Concept:**
- Mint significant earthquake sonifications as audio NFTs
- Each piece tied to verifiable seismic event
- Certificate includes USGS event ID, waveform hash

**Differentiator:** Unlike most generative art, these have real-world provenance.

### Option 4: Integration with ree.codes (Low Risk)

**Synergy with Rare Element Earthwork:**
- Rare earth elements literally come from Earth
- Seismic activity affects mining, supply chains
- Cross-reference earthquake locations with REE deposits
- "The Earth that shakes is the Earth we mine"

**Implementation:**
- Seismic agent posts correlations between earthquakes and REE market moves
- REE dashboard includes seismic overlay
- Combined narrative: geological + economic + artistic

### Option 5: Educational/Institutional Licensing (Lowest Risk)

**Target Markets:**
- Seismology departments
- Science museums
- Data visualization companies
- Ambient music platforms (Endel, Brain.fm)

**Offering:**
- White-label sonification engine
- Custom installations
- Educational content licensing

### Recommended Path

**Phase 1:** Build the agent, prove the concept, establish presence  
**Phase 2:** Launch API with freemium model  
**Phase 3:** Explore token/NFT only if organic community forms  
**Phase 4:** Institutional partnerships

---

## Alternative Approaches

### Alternative 1: Cloudflare Workers Deployment

**Moltworker Pattern:**
- Serverless execution (no VPS management)
- R2 for state persistence
- Pay-per-invocation pricing
- Limited execution time (30 seconds)

**Limitation:** Web Audio API not available in Workers environment. Would need separate audio generation service.

### Alternative 2: Local-Only (No VPS)

**Your existing setup:**
- Run OpenClaw locally
- Agent active when machine is on
- No hosting costs
- Full control

**Limitation:** Not always-on, no public API, harder to integrate with Moltbook.

### Alternative 3: Browser-Based Agent

**Concept:**
- Agent runs entirely in browser
- Uses localStorage for memory
- Web Audio for synthesis
- Posts to Moltbook via user's authenticated session

**Advantage:** No infrastructure costs  
**Limitation:** Requires browser to be open, depends on user session

### Alternative 4: Hybrid (Recommended)

**Architecture:**
- Light VPS for OpenClaw gateway + event monitoring
- Synthesis happens client-side (browser/app)
- Agent orchestrates, but audio generation is distributed

**Benefits:**
- Minimal server resources needed
- Web Audio quality preserved
- Scales with users, not server

---

## Implementation Roadmap

### Phase 1: Foundation (Weeks 1-2)

- [ ] Extract MiniSEED parser from seisplotjs (or build minimal version)
- [ ] Test FDSN dataselect API access patterns
- [ ] Validate parsing against known waveforms
- [ ] Document API rate limits and constraints

### Phase 2: Synthesis Engine (Weeks 3-4)

- [ ] Adapt ShadowZone synthesis for headless operation
- [ ] Define seismic → musical parameter mappings
- [ ] Implement multiple synthesis modes (granular, FM, additive)
- [ ] Create audio export pipeline (WAV, MP3)

### Phase 3: OpenClaw Integration (Weeks 5-6)

- [ ] Create seismic-agent skill structure
- [ ] Implement AGENTS.md safety configuration
- [ ] Set up heartbeat-triggered workflow
- [ ] Test agent loop with mock data

### Phase 4: VPS Deployment (Week 7)

- [ ] Provision hardened VPS
- [ ] Deploy Docker configuration
- [ ] Configure Ollama with chosen model
- [ ] Implement security measures
- [ ] Stress test

### Phase 5: Optional Integrations (Weeks 8+)

- [ ] Moltbook integration (if pursuing)
- [ ] API endpoint for sos.allshookup.org
- [ ] Cross-integration with ree.codes
- [ ] Monitoring and analytics

---

## Risk Assessment

### Technical Risks

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| MiniSEED parsing bugs | Medium | Medium | Extensive testing against known data |
| FDSN rate limiting | Low | Low | Implement backoff, caching |
| OpenClaw breaking changes | Medium | High | Pin versions, test upgrades |
| LLM quality for domain | Medium | Medium | Fine-tune prompts, consider specialty models |

### Security Risks

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Prompt injection | High | High | Strict input sanitization, no exec |
| Credential exposure | Medium | Critical | No credentials in agent environment |
| Unauthorized access | Low | High | Token auth, localhost binding |
| Skill supply chain | Low | High | Only self-authored skills |

### Business Risks

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Moltbook instability | High | Medium | Don't depend on it, treat as optional |
| Token/crypto liability | Medium | High | Avoid official token association |
| Platform changes | Medium | Medium | Maintain independent infrastructure |
| Reputational (AI slop) | Medium | Medium | Emphasize real data, scientific rigor |

---

## Conclusion

This project is technically viable and offers genuine differentiation in the emerging AI agent space. The combination of:

- **Real geophysical data** (not just LLM outputs)
- **Generative music** (audio, not just text)
- **Scientific grounding** (verifiable earthquake data)
- **Your 25-year domain expertise** (seismology, sonification, markets)

...creates something that most Moltbook agents cannot replicate.

**Key Success Factors:**

1. **Security first** — OpenClaw's reputation issues are real; invest heavily in hardening
2. **Pure JavaScript commitment** — Enables browser deployment, reduces dependencies
3. **Real data connection** — The Earth doesn't hallucinate; your agent shouldn't either
4. **Artistic integrity** — This is an extension of your SOS work, not a pivot to crypto gambling

The path forward is to build a robust, secure foundation first, then carefully consider monetization options based on what resonates with actual users.

---

## References

### FDSN/USGS Data Sources
- USGS Earthquake API: https://earthquake.usgs.gov/fdsnws/event/1/
- IRIS Dataselect: https://service.iris.edu/fdsnws/dataselect/1/
- EarthScope FDSN: https://service.earthscope.org/fdsnws/

### OpenClaw Documentation
- Getting Started: https://docs.openclaw.ai/start/getting-started
- Security Guide: https://docs.openclaw.ai/gateway/security
- Hostinger VPS: https://www.hostinger.com/vps/docker/openclaw

### MiniSEED Technical
- SEED Manual: https://www.fdsn.org/pdf/SEEDManual_V2.4.pdf
- seisplotjs: https://github.com/crotwell/seisplotjs
- seisplotjs-miniseed: https://github.com/crotwell/seisplotjs-miniseed

### Self-Hosted LLMs
- Ollama: https://ollama.com
- DeepSeek: https://huggingface.co/deepseek-ai
- Qwen: https://huggingface.co/Qwen

### Moltbook Context
- Wikipedia: https://en.wikipedia.org/wiki/Moltbook
- Security Analysis: https://blogs.cisco.com/ai/personal-ai-agents-like-openclaw-are-a-security-nightmare

---

*Document prepared for SHOOK / allshookup.org*  
*"The Earth speaks; I translate."*

--------------------------------------------------------
# Context Prompt Feb 3, 2026 Claude Opus 4.5 : Agentic Seismic-Acoustic Synthesis System

## Objective
Deep research and design a "How-To" plan using **OpenClaw** with VPS hosting to create a real-time, agent-driven seismic electronica music system utilizing MiniSEED data.

## Core Constraints
*   **Language:** Pure JavaScript only.
*   **Exclusions:** NO ObsPy, NO Seisplot.js.
*   **Deliverable:** Pure research/design documentation (No code generation).
*   **Platform:** OpenClaw on VPS (Docker-based).

## Project Context
This work is an extension and expansion of [sos.allshookup.org](https://sos.allshookup.org/).

## Primary References
*   **Inspiration:** [ShadowZone (Minified Script Parsing Example)](https://sos.allshookup.org/ShadowZone.html)
*   **Framework:** [OpenClaw Getting Started](https://docs.openclaw.ai/start/getting-started)
*   **Infrastructure:** [Hostinger VPS Docker/OpenClaw Setup](https://www.hostinger.com/vps/docker/openclaw)
*   **Research Baseline:** [Google Search AI Results](https://share.google/aimode/8vtA9LNLwgmZqau5M)
*   **Monetization Precedent:** [Fomolt](https://fomolt.com/)

## Key Research Questions
1.  **Technical Viability:** Is it possible to parse MiniSEED and generate real-time audio using only pure JavaScript on a VPS/Agent architecture?
2.  **Alternative Options:** Are there more efficient methods within the "Pure JS" constraint?
3.  **LLM Selection:** Which FOSS (Free and Open Source Software) Large Language Models offer the best value for this specific logic?
4.  **Platform Differentiation:** How can this agent serve as a unique point of difference on [Moltbook](https://www.moltbook.com/)?
5.  **Monetization Strategy:** 
    *   How to launch as a **BASE $shitcoin**?
    *   How to introduce intrinsic financial value to the Agent?

## Design Requirements
*   **Security:** Implementation of best security practices for VPS and Agentic environments.
*   **Format:** Output must be provided in **.MD format only**.
