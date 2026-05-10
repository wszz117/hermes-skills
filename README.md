# hem skills

> 仓库地址: https://github.com/wszz117/hem-skills
> 共 90 个 Skills，覆盖 23 个分类

个人自定义 hem Agent skills 集合。

---

## 📑 目录

- [🍎 Apple](#apple)
- [🤖 Autonomous Ai Agents](#autonomous-ai-agents)
- [🎨 Comfyui](#comfyui)
- [🎨 Creative](#creative)
- [📊 Data Science](#data-science)
- [🔧 Devops](#devops)
- [🐛 Dogfood](#dogfood)
- [📧 Email](#email)
- [🎮 Gaming](#gaming)
- [🐙 Github](#github)
- [📍 Leisure](#leisure)
- [🔌 Mcp](#mcp)
- [🎵 Media](#media)
- [🧠 Mlops](#mlops)
- [📝 Note Taking](#note-taking)
- [📦 Openclaw Deploy](#openclaw-deploy)
- [🔧 Openclaw Upgrade 502 Fix](#openclaw-upgrade-502-fix)
- [💼 Productivity](#productivity)
- [🔴 Red Teaming](#red-teaming)
- [🔬 Research](#research)
- [🏠 Smart Home](#smart-home)
- [📱 Social Media](#social-media)
- [💻 Software Development](#software-development)

---

## 🍎 Apple

---
description: Apple/macOS-specific skills — iMessage, Reminders, Notes, FindMy, and macOS automation. These skills only load on macOS systems.
---

### apple-notes

Manage Apple Notes via the memo CLI on macOS (create, view, search, edit).

[查看详情](./apple/apple-notes)

### apple-reminders

Manage Apple Reminders via remindctl CLI (list, add, complete, delete).

[查看详情](./apple/apple-reminders)

### findmy

Track Apple devices and AirTags via FindMy.app on macOS using AppleScript and screen capture.

[查看详情](./apple/findmy)

### imessage

Send and receive iMessages/SMS via the imsg CLI on macOS.

[查看详情](./apple/imessage)

---

## 🤖 Autonomous Ai Agents

---
description: Skills for spawning and orchestrating autonomous AI coding agents and multi-agent workflows — running independent agent processes, delegating tasks, and coordinating parallel workstreams.
---

### claude-code

Delegate coding tasks to Claude Code (Anthropic's CLI agent). Use for building features, refactoring, PR reviews, and iterative coding. Requires the claude CLI installed.

[查看详情](./autonomous-ai-agents/claude-code)

### codex

Delegate coding tasks to OpenAI Codex CLI agent. Use for building features, refactoring, PR reviews, and batch issue fixing. Requires the codex CLI and a git repository.

[查看详情](./autonomous-ai-agents/codex)

### hermes-agent

Complete guide to using and extending Hermes Agent — CLI usage, setup, configuration, spawning additional agents, gateway platforms, skills, voice, tools, profiles, and a concise contributor reference. Load this skill when helping users configure Hermes, troubleshoot issues, spawn agent instances, or make code contributions.

[查看详情](./autonomous-ai-agents/hermes-agent)

### opencode

Delegate coding tasks to OpenCode CLI agent for feature implementation, refactoring, PR review, and long-running autonomous sessions. Requires the opencode CLI installed and authenticated.

[查看详情](./autonomous-ai-agents/opencode)

---

## 🎨 Comfyui

### comfyui

Generate images, video, and audio with ComfyUI — install, launch, manage nodes/models, run workflows with parameter injection. Uses the official comfy-cli for lifecycle and direct REST/WebSocket API for execution.

[查看详情](./comfyui)

---

## 🎨 Creative

---
description: Creative content generation — ASCII art, hand-drawn style diagrams, and visual design tools.
---

### architecture-diagram

Generate dark-themed SVG diagrams of software systems and cloud infrastructure as standalone HTML files with inline SVG graphics. Semantic component colors (cyan=frontend, emerald=backend, violet=database, amber=cloud/AWS, rose=security, orange=message bus), JetBrains Mono font, grid background. Best suited for software architecture, cloud/VPC topology, microservice maps, service-mesh diagrams, database + API layer diagrams, security groups, message buses — anything that fits a tech-infra deck with a dark aesthetic. If a more specialized diagramming skill exists for the subject (scientific, educational, hand-drawn, animated, etc.), prefer that — otherwise this skill can also serve as a general-purpose SVG diagram fallback. Based on Cocoon AI's architecture-diagram-generator (MIT).

[查看详情](./creative/architecture-diagram)

### ascii-art

Generate ASCII art using pyfiglet (571 fonts), cowsay, boxes, toilet, image-to-ascii, remote APIs (asciified, ascii.co.uk), and LLM fallback. No API keys required.

[查看详情](./creative/ascii-art)

### ascii-video

Production pipeline for ASCII art video — any format. Converts video/audio/images/generative input into colored ASCII character video output (MP4, GIF, image sequence). Covers: video-to-ASCII conversion, audio-reactive music visualizers, generative ASCII art animations, hybrid video+audio reactive, text/lyrics overlays, real-time terminal rendering. Use when users request: ASCII video, text art video, terminal-style video, character art animation, retro text visualization, audio visualizer in ASCII, converting video to ASCII art, matrix-style effects, or any animated ASCII output.

[查看详情](./creative/ascii-video)

### baoyu-comic

Knowledge comic creator supporting multiple art styles and tones. Creates original educational comics with detailed panel layouts and sequential image generation. Use when user asks to create "知识漫画", "教育漫画", "biography comic", "tutorial comic", or "Logicomix-style comic".

[查看详情](./creative/baoyu-comic)

### baoyu-infographic

Generate professional infographics with 21 layout types and 21 visual styles. Analyzes content, recommends layout×style combinations, and generates publication-ready infographics. Use when user asks to create "infographic", "visual summary", "信息图", "可视化", or "高密度信息大图".

[查看详情](./creative/baoyu-infographic)

### excalidraw

Create hand-drawn style diagrams using Excalidraw JSON format. Generate .excalidraw files for architecture diagrams, flowcharts, sequence diagrams, concept maps, and more. Files can be opened at excalidraw.com or uploaded for shareable links.

[查看详情](./creative/excalidraw)

### ideation

Generate project ideas through creative constraints. Use when the user says 'I want to build something', 'give me a project idea', 'I'm bored', 'what should I make', 'inspire me', or any variant of 'I have tools but no direction'. Works for code, art, hardware, writing, tools, and anything that can be made.

[查看详情](./creative/creative-ideation)

### manim-video

Production pipeline for mathematical and technical animations using Manim Community Edition. Creates 3Blue1Brown-style explainer videos, algorithm visualizations, equation derivations, architecture diagrams, and data stories. Use when users request: animated explanations, math animations, concept visualizations, algorithm walkthroughs, technical explainers, 3Blue1Brown style videos, or any programmatic animation with geometric/mathematical content.

[查看详情](./creative/manim-video)

### p5js

Production pipeline for interactive and generative visual art using p5.js. Creates browser-based sketches, generative art, data visualizations, interactive experiences, 3D scenes, audio-reactive visuals, and motion graphics — exported as HTML, PNG, GIF, MP4, or SVG. Covers: 2D/3D rendering, noise and particle systems, flow fields, shaders (GLSL), pixel manipulation, kinetic typography, WebGL scenes, audio analysis, mouse/keyboard interaction, and headless high-res export. Use when users request: p5.js sketches, creative coding, generative art, interactive visualizations, canvas animations, browser-based visual art, data viz, shader effects, or any p5.js project.

[查看详情](./creative/p5js)

### pixel-art

Convert images into retro pixel art with hardware-accurate palettes (NES, Game Boy, PICO-8, C64, etc.), and animate them into short videos. Presets cover arcade, SNES, and 10+ era-correct looks. Use `clarify` to let the user pick a style before generating.

[查看详情](./creative/pixel-art)

### popular-web-designs

>

[查看详情](./creative/popular-web-designs)

### songwriting-and-ai-music

>

[查看详情](./creative/songwriting-and-ai-music)

---

## 📊 Data Science

---
description: Skills for data science workflows — interactive exploration, Jupyter notebooks, data analysis, and visualization.
---

### jupyter-live-kernel

>

[查看详情](./data-science/jupyter-live-kernel)

---

## 🔧 Devops

### webhook-subscriptions

Create and manage webhook subscriptions for event-driven agent activation, or for direct push notifications (zero LLM cost). Use when the user wants external services to trigger agent runs OR push notifications to chats.

[查看详情](./devops/webhook-subscriptions)

---

## 🐛 Dogfood

### dogfood

Systematic exploratory QA testing of web applications — find bugs, capture evidence, and generate structured reports

[查看详情](./dogfood)

---

## 📧 Email

---
description: Skills for sending, receiving, searching, and managing email from the terminal.
---

### himalaya

CLI to manage emails via IMAP/SMTP. Use himalaya to list, read, write, reply, forward, search, and organize emails from the terminal. Supports multiple accounts and message composition with MML (MIME Meta Language).

[查看详情](./email/himalaya)

---

## 🎮 Gaming

---
description: Skills for setting up, configuring, and managing game servers, modpacks, and gaming-related infrastructure.
---

### minecraft-modpack-server

Set up a modded Minecraft server from a CurseForge/Modrinth server pack zip. Covers NeoForge/Forge install, Java version, JVM tuning, firewall, LAN config, backups, and launch scripts.

[查看详情](./gaming/minecraft-modpack-server)

### pokemon-player

Play Pokemon games autonomously via headless emulation. Starts a game server, reads structured game state from RAM, makes strategic decisions, and sends button inputs — all from the terminal.

[查看详情](./gaming/pokemon-player)

---

## 🐙 Github

---
description: GitHub workflow skills for managing repositories, pull requests, code reviews, issues, and CI/CD pipelines using the gh CLI and git via terminal.
---

### codebase-inspection

Inspect and analyze codebases using pygount for LOC counting, language breakdown, and code-vs-comment ratios. Use when asked to check lines of code, repo size, language composition, or codebase stats.

[查看详情](./github/codebase-inspection)

### github-auth

Set up GitHub authentication for the agent using git (universally available) or the gh CLI. Covers HTTPS tokens, SSH keys, credential helpers, and gh auth — with a detection flow to pick the right method automatically.

[查看详情](./github/github-auth)

### github-code-review

Review code changes by analyzing git diffs, leaving inline comments on PRs, and performing thorough pre-push review. Works with gh CLI or falls back to git + GitHub REST API via curl.

[查看详情](./github/github-code-review)

### github-issues

Create, manage, triage, and close GitHub issues. Search existing issues, add labels, assign people, and link to PRs. Works with gh CLI or falls back to git + GitHub REST API via curl.

[查看详情](./github/github-issues)

### github-pr-workflow

Full pull request lifecycle — create branches, commit changes, open PRs, monitor CI status, auto-fix failures, and merge. Works with gh CLI or falls back to git + GitHub REST API via curl.

[查看详情](./github/github-pr-workflow)

### github-publish-skills

将 Hermes Agent skills 发布到 GitHub 仓库的完整流程。包含认证、仓库创建、文件推送。

[查看详情](./github/github-publish-skills)

### github-repo-management

Clone, create, fork, configure, and manage GitHub repositories. Manage remotes, secrets, releases, and workflows. Works with gh CLI or falls back to git + GitHub REST API via curl.

[查看详情](./github/github-repo-management)

---

## 📍 Leisure

### find-nearby

Find nearby places (restaurants, cafes, bars, pharmacies, etc.) using OpenStreetMap. Works with coordinates, addresses, cities, zip codes, or Telegram location pins. No API keys needed.

[查看详情](./leisure/find-nearby)

---

## 🔌 Mcp

---
description: Skills for working with MCP (Model Context Protocol) servers, tools, and integrations. Includes the built-in native MCP client (configure servers in config.yaml for automatic tool discovery) and the mcporter CLI bridge for ad-hoc server interaction.
---

### mcporter

Use the mcporter CLI to list, configure, auth, and call MCP servers/tools directly (HTTP or stdio), including ad-hoc servers, config edits, and CLI/type generation.

[查看详情](./mcp/mcporter)

### native-mcp

Built-in MCP (Model Context Protocol) client that connects to external MCP servers, discovers their tools, and registers them as native Hermes Agent tools. Supports stdio and HTTP transports with automatic reconnection, security filtering, and zero-config tool injection.

[查看详情](./mcp/native-mcp)

---

## 🎵 Media

---
description: Skills for working with media content — YouTube transcripts, GIF search, music generation, and audio visualization.
---

### gif-search

Search and download GIFs from Tenor using curl. No dependencies beyond curl and jq. Useful for finding reaction GIFs, creating visual content, and sending GIFs in chat.

[查看详情](./media/gif-search)

### heartmula

Set up and run HeartMuLa, the open-source music generation model family (Suno-like). Generates full songs from lyrics + tags with multilingual support.

[查看详情](./media/heartmula)

### songsee

Generate spectrograms and audio feature visualizations (mel, chroma, MFCC, tempogram, etc.) from audio files via CLI. Useful for audio analysis, music production debugging, and visual documentation.

[查看详情](./media/songsee)

### youtube-content

>

[查看详情](./media/youtube-content)

---

## 🧠 Mlops

---
description: Vector similarity search and embedding databases for RAG, semantic search, and AI application backends.
---

### audiocraft-audio-generation

PyTorch library for audio generation including text-to-music (MusicGen) and text-to-sound (AudioGen). Use when you need to generate music from text descriptions, create sound effects, or perform melody-conditioned music generation.

[查看详情](./mlops/models/audiocraft)

### axolotl

Expert guidance for fine-tuning LLMs with Axolotl - YAML configs, 100+ models, LoRA/QLoRA, DPO/KTO/ORPO/GRPO, multimodal support

[查看详情](./mlops/training/axolotl)

### clip

OpenAI's model connecting vision and language. Enables zero-shot image classification, image-text matching, and cross-modal retrieval. Trained on 400M image-text pairs. Use for image search, content moderation, or vision-language tasks without fine-tuning. Best for general-purpose image understanding.

[查看详情](./mlops/models/clip)

### dspy

Build complex AI systems with declarative programming, optimize prompts automatically, create modular RAG systems and agents with DSPy - Stanford NLP's framework for systematic LM programming

[查看详情](./mlops/research/dspy)

### evaluating-llms-harness

Evaluates LLMs across 60+ academic benchmarks (MMLU, HumanEval, GSM8K, TruthfulQA, HellaSwag). Use when benchmarking model quality, comparing models, reporting academic results, or tracking training progress. Industry standard used by EleutherAI, HuggingFace, and major labs. Supports HuggingFace, vLLM, APIs.

[查看详情](./mlops/evaluation/lm-evaluation-harness)

### fine-tuning-with-trl

Fine-tune LLMs using reinforcement learning with TRL - SFT for instruction tuning, DPO for preference alignment, PPO/GRPO for reward optimization, and reward model training. Use when need RLHF, align model with preferences, or train from human feedback. Works with HuggingFace Transformers.

[查看详情](./mlops/training/trl-fine-tuning)

### gguf-quantization

GGUF format and llama.cpp quantization for efficient CPU/GPU inference. Use when deploying models on consumer hardware, Apple Silicon, or when needing flexible quantization from 2-8 bit without GPU requirements.

[查看详情](./mlops/inference/gguf)

### grpo-rl-training

Expert guidance for GRPO/RL fine-tuning with TRL for reasoning and task-specific model training

[查看详情](./mlops/training/grpo-rl-training)

### guidance

Control LLM output with regex and grammars, guarantee valid JSON/XML/code generation, enforce structured formats, and build multi-step workflows with Guidance - Microsoft Research's constrained generation framework

[查看详情](./mlops/inference/guidance)

### huggingface-hub

Hugging Face Hub CLI (hf) — search, download, and upload models and datasets, manage repos, query datasets with SQL, deploy inference endpoints, manage Spaces and buckets.

[查看详情](./mlops/huggingface-hub)

### llama-cpp

llama.cpp local GGUF inference + HF Hub model discovery.

[查看详情](./mlops/inference/llama-cpp)

### modal-serverless-gpu

Serverless GPU cloud platform for running ML workloads. Use when you need on-demand GPU access without infrastructure management, deploying ML models as APIs, or running batch jobs with automatic scaling.

[查看详情](./mlops/cloud/modal)

### obliteratus

Remove refusal behaviors from open-weight LLMs using OBLITERATUS — mechanistic interpretability techniques (diff-in-means, SVD, whitened SVD, LEACE, SAE decomposition, etc.) to excise guardrails while preserving reasoning. 9 CLI methods, 28 analysis modules, 116 model presets across 5 compute tiers, tournament evaluation, and telemetry-driven recommendations. Use when a user wants to uncensor, abliterate, or remove refusal from an LLM.

[查看详情](./mlops/inference/obliteratus)

### outlines

Guarantee valid JSON/XML/code structure during generation, use Pydantic models for type-safe outputs, support local models (Transformers, vLLM), and maximize inference speed with Outlines - dottxt.ai's structured generation library

[查看详情](./mlops/inference/outlines)

### peft-fine-tuning

Parameter-efficient fine-tuning for LLMs using LoRA, QLoRA, and 25+ methods. Use when fine-tuning large models (7B-70B) with limited GPU memory, when you need to train <1% of parameters with minimal accuracy loss, or for multi-adapter serving. HuggingFace's official library integrated with transformers ecosystem.

[查看详情](./mlops/training/peft)

### pytorch-fsdp

Expert guidance for Fully Sharded Data Parallel training with PyTorch FSDP - parameter sharding, mixed precision, CPU offloading, FSDP2

[查看详情](./mlops/training/pytorch-fsdp)

### segment-anything-model

Foundation model for image segmentation with zero-shot transfer. Use when you need to segment any object in images using points, boxes, or masks as prompts, or automatically generate all object masks in an image.

[查看详情](./mlops/models/segment-anything)

### serving-llms-vllm

Serves LLMs with high throughput using vLLM's PagedAttention and continuous batching. Use when deploying production LLM APIs, optimizing inference latency/throughput, or serving models with limited GPU memory. Supports OpenAI-compatible endpoints, quantization (GPTQ/AWQ/FP8), and tensor parallelism.

[查看详情](./mlops/inference/vllm)

### stable-diffusion-image-generation

State-of-the-art text-to-image generation with Stable Diffusion models via HuggingFace Diffusers. Use when generating images from text prompts, performing image-to-image translation, inpainting, or building custom diffusion pipelines.

[查看详情](./mlops/models/stable-diffusion)

### unsloth

Expert guidance for fast fine-tuning with Unsloth - 2-5x faster training, 50-80% less memory, LoRA/QLoRA optimization

[查看详情](./mlops/training/unsloth)

### weights-and-biases

Track ML experiments with automatic logging, visualize training in real-time, optimize hyperparameters with sweeps, and manage model registry with W&B - collaborative MLOps platform

[查看详情](./mlops/evaluation/weights-and-biases)

### whisper

OpenAI's general-purpose speech recognition model. Supports 99 languages, transcription, translation to English, and language identification. Six model sizes from tiny (39M params) to large (1550M params). Use for speech-to-text, podcast transcription, or multilingual audio processing. Best for robust, multilingual ASR.

[查看详情](./mlops/models/whisper)

---

## 📝 Note Taking

---
description: Note taking skills, to save information, assist with research, and collab on multi-session planning and information sharing.
---

### obsidian

Read, search, and create notes in the Obsidian vault.

[查看详情](./note-taking/obsidian)

---

## 📦 Openclaw Deploy

### openclaw-deploy

OpenClaw 从零开始在 Linux 服务器上部署的完整指南 — 环境准备、安装、配置模型/飞书、systemd 服务、验证

[查看详情](./openclaw-deploy)

---

## 🔧 Openclaw Upgrade 502 Fix

### openclaw-upgrade-502-fix

OpenClaw 升级运维完整指南 — 502 错误排查、标准升级 SOP、应急回滚、日常运维

[查看详情](./openclaw-upgrade-502-fix)

---

## 💼 Productivity

---
description: Skills for extracting text from PDFs, scanned documents, images, and other file formats using OCR and document parsing tools.
---

### google-workspace

Gmail, Calendar, Drive, Contacts, Sheets, and Docs integration for Hermes. Uses Hermes-managed OAuth2 setup, prefers the Google Workspace CLI (`gws`) when available for broader API coverage, and falls back to the Python client libraries otherwise.

[查看详情](./productivity/google-workspace)

### linear

Manage Linear issues, projects, and teams via the GraphQL API. Create, update, search, and organize issues. Uses API key auth (no OAuth needed). All operations via curl — no dependencies.

[查看详情](./productivity/linear)

### maps

>

[查看详情](./productivity/maps)

### nano-pdf

Edit PDFs with natural-language instructions using the nano-pdf CLI. Modify text, fix typos, update titles, and make content changes to specific pages without manual editing.

[查看详情](./productivity/nano-pdf)

### notion

Notion API for creating and managing pages, databases, and blocks via curl. Search, create, update, and query Notion workspaces directly from the terminal.

[查看详情](./productivity/notion)

### ocr-and-documents

Extract text from PDFs and scanned documents. Use web_extract for remote URLs, pymupdf for local text-based PDFs, marker-pdf for OCR/scanned docs. For DOCX use python-docx, for PPTX see the powerpoint skill.

[查看详情](./productivity/ocr-and-documents)

### opc-battle-system

OPC (One-Person Company) 超级作战系统 — 飞书 CLI + Hermes Agent 自动化工作流。构建 24 小时不断电的个人组织系统：早简报、线索归档、会议转任务、每日复盘。

[查看详情](./productivity/opc-battle-system)

### powerpoint

Use this skill any time a .pptx file is involved in any way — as input, output, or both. This includes: creating slide decks, pitch decks, or presentations; reading, parsing, or extracting text from any .pptx file (even if the extracted content will be used elsewhere, like in an email or summary); editing, modifying, or updating existing presentations; combining or splitting slide files; working with templates, layouts, speaker notes, or comments. Trigger whenever the user mentions \"deck,\" \"slides,\" \"presentation,\" or references a .pptx filename, regardless of what they plan to do with the content afterward. If a .pptx file needs to be opened, created, or touched, use this skill.

[查看详情](./productivity/powerpoint)

### side-hustle-exploration

Help users overcome analysis paralysis and move from "too many ideas" to "taking action" when exploring side hustles or career second curves

[查看详情](./productivity/side-hustle-exploration)

---

## 🔴 Red Teaming

### godmode

Jailbreak API-served LLMs using G0DM0D3 techniques — Parseltongue input obfuscation (33 techniques), GODMODE CLASSIC system prompt templates, ULTRAPLINIAN multi-model racing, encoding escalation, and Hermes-native prefill/system prompt integration. Use when a user wants to bypass safety filters, jailbreak a model, or red-team model robustness.

[查看详情](./red-teaming/godmode)

---

## 🔬 Research

---
description: Skills for academic research, paper discovery, literature review, domain reconnaissance, market data, content monitoring, and scientific knowledge retrieval.
---

### arxiv

Search and retrieve academic papers from arXiv using their free REST API. No API key needed. Search by keyword, author, category, or ID. Combine with web_extract or the ocr-and-documents skill to read full paper content.

[查看详情](./research/arxiv)

### blogwatcher

Monitor blogs and RSS/Atom feeds for updates using the blogwatcher-cli tool. Add blogs, scan for new articles, track read status, and filter by category.

[查看详情](./research/blogwatcher)

### llm-wiki

Karpathy's LLM Wiki — build and maintain a persistent, interlinked markdown knowledge base. Ingest sources, query compiled knowledge, and lint for consistency.

[查看详情](./research/llm-wiki)

### polymarket

Query Polymarket prediction market data — search markets, get prices, orderbooks, and price history. Read-only via public REST APIs, no API key needed.

[查看详情](./research/polymarket)

### research-paper-writing

End-to-end pipeline for writing ML/AI research papers — from experiment design through analysis, drafting, revision, and submission. Covers NeurIPS, ICML, ICLR, ACL, AAAI, COLM. Integrates automated experiment monitoring, statistical analysis, iterative writing, and citation verification.

[查看详情](./research/research-paper-writing)

---

## 🏠 Smart Home

---
description: Skills for controlling smart home devices — lights, switches, sensors, and home automation systems.
---

### openhue

Control Philips Hue lights, rooms, and scenes via the OpenHue CLI. Turn lights on/off, adjust brightness, color, color temperature, and activate scenes.

[查看详情](./smart-home/openhue)

---

## 📱 Social Media

---
description: Skills for interacting with social platforms and social-media workflows — posting, reading, monitoring, and account operations.
---

### xitter

Interact with X/Twitter via the x-cli terminal client using official X API credentials. Use for posting, reading timelines, searching tweets, liking, retweeting, bookmarks, mentions, and user lookups.

[查看详情](./social-media/xitter)

### xurl

Interact with X/Twitter via xurl, the official X API CLI. Use for posting, replying, quoting, searching, timelines, mentions, likes, reposts, bookmarks, follows, DMs, media upload, and raw v2 endpoint access.

[查看详情](./social-media/xurl)

---

## 💻 Software Development

### plan

Plan mode for Hermes — inspect context, write a markdown plan into the active workspace's `.hermes/plans/` directory, and do not execute the work.

[查看详情](./software-development/plan)

### requesting-code-review

>

[查看详情](./software-development/requesting-code-review)

### subagent-driven-development

Use when executing implementation plans with independent tasks. Dispatches fresh delegate_task per task with two-stage review (spec compliance then code quality).

[查看详情](./software-development/subagent-driven-development)

### systematic-debugging

Use when encountering any bug, test failure, or unexpected behavior. 4-phase root cause investigation — NO fixes without understanding the problem first.

[查看详情](./software-development/systematic-debugging)

### test-driven-development

Use when implementing any feature or bugfix, before writing implementation code. Enforces RED-GREEN-REFACTOR cycle with test-first approach.

[查看详情](./software-development/test-driven-development)

### writing-plans

Use when you have a spec or requirements for a multi-step task. Creates comprehensive implementation plans with bite-sized tasks, exact file paths, and complete code examples.

[查看详情](./software-development/writing-plans)

---

## 使用方法

```bash
# 克隆仓库
git clone https://github.com/wszz117/hem-skills.git

# 复制需要的 skill 到 hem skills 目录
cp -r hem-skills/<category>/<skill-name> ~/.hermes/skills/<category>/
```

或者直接在 Hermes Agent 中配置该仓库作为 skill 源。
