# HotOrBot Vid — Product & Technical Specification
### Version 0.1 | February 2026
### Author: Amanda Bradford / Mandyee

---

## Table of Contents
1. [Product Overview](#1-product-overview)
2. [User Experience Flow](#2-user-experience-flow)
3. [Core Features (MVP)](#3-core-features-mvp)
4. [Technical Architecture](#4-technical-architecture)
5. [Real-Time Video Pipeline](#5-real-time-video-pipeline)
6. [AI Agent System](#6-ai-agent-system)
7. [Infrastructure & Stack](#7-infrastructure--stack)
8. [Data Model](#8-data-model)
9. [Latency Budget](#9-latency-budget)
10. [Cost Model](#10-cost-model)
11. [Security & Trust](#11-security--trust)
12. [MVP Scope & Phases](#12-mvp-scope--phases)
13. [Open Questions](#13-open-questions)

---

## 1. Product Overview

**HotOrBot** is a web-based dating game where users go on short live video dates — but they never know if they're talking to the real person or that person's AI agent.

After each date, the user votes **HOT or NOT**. Then the reveal happens: were they talking to the real human, or their AI twin? Regardless of the outcome, the user gets a **second chance** — now that they know the truth, do they want to match?

**One-liner:** "A live video dating game where you don't know if you're talking to the person or their AI — until after you decide."

**Platform:** Web application (desktop & mobile browsers). No native app for MVP.

**Why web-first:**
- Zero friction — no App Store approval, no downloads
- Faster iteration cycles (deploy in minutes, not days)
- WebRTC works natively in all modern browsers
- Camera/mic permissions are standard web APIs
- Easier to share links virally (TikTok → link in bio → playing in 30 seconds)
- Can always wrap in a PWA or native shell later

---

## 2. User Experience Flow

### 2.1 Onboarding (First Time)

```
Landing Page → Sign Up → Create Profile → Train Your Agent → Ready to Play
```

**Sign Up:**
- Email/Google/Apple auth (no phone number required for MVP)
- Basic profile: name, age, gender, who you're interested in, city
- Upload 3-5 photos (used for your profile AND to generate your avatar)

**Train Your Agent (the magic moment):**
- Take 2 selfies: face forward + turn to the side (gives avatar model 3D face geometry)
- Record a voice sample: read the prompt *"Sorry I'm not home right now, I'm walking through a spiderweb. Leave a message and I'll call you back."* (~7 seconds, enough for instant voice clone)
- Answer 20 rapid-fire personality questions (humor style, deal-breakers, interests)
- Choose what happens when you're not there:
  - **Default away message:** Your avatar plays the spiderweb greeting. Automatic, zero effort. Everyone gets this.
  - **Custom away message:** Record your own away message for your avatar to play.
  - **Full AI agent (opt-in):** Your AI shows up AS you and has a real free-flowing conversation. Unvetted, unpredictable — that's the fun.
- Optional: connect Spotify, import a few text convos for style matching
- System generates: voice clone + avatar model + personality profile (processing in background)
- User gets a preview when ready ("Here's your AI twin — say hi!") and can redo

**Time to play:** ~3 minutes from landing page to first video date.

### 2.2 The Game Loop

```
┌─────────────────────────────────────────────────────┐
│                    LOBBY                              │
│  "Finding your next date..."                         │
│  [matched in 10-30 seconds]                          │
└──────────────────────┬──────────────────────────────┘
                       ▼
┌─────────────────────────────────────────────────────┐
│                 VIDEO DATE                            │
│  5-minute live video call                            │
│  You ←→ Real Person OR Their AI Agent                │
│  [timer counting down]                               │
│  [conversation prompts if awkward silence]            │
└──────────────────────┬──────────────────────────────┘
                       ▼
┌─────────────────────────────────────────────────────┐
│                THE VOTE                               │
│                                                       │
│  "Did you feel the vibe?"                            │
│                                                       │
│      [🔥 HOT]          [❌ NOT]                      │
│                                                       │
│  "Was that real or AI?"                              │
│                                                       │
│      [👤 REAL]          [🤖 BOT]                     │
│                                                       │
└──────────────────────┬──────────────────────────────┘
                       ▼
┌─────────────────────────────────────────────────────┐
│              THE REVEAL 🎭                            │
│                                                       │
│  Dramatic animation... "IT WAS..."                   │
│                                                       │
│     🤖 THE BOT!  (or)  👤 THE REAL PERSON!          │
│                                                       │
│  You voted: HOT ✅   You guessed: BOT ❌             │
│                                                       │
│  [Share this reveal 📱]                              │
│                                                       │
└──────────────────────┬──────────────────────────────┘
                       ▼
┌─────────────────────────────────────────────────────┐
│            SECOND CHANCE                              │
│                                                       │
│  "Now that you know... want to match?"               │
│                                                       │
│      [❤️ YES, MATCH]     [👋 PASS]                   │
│                                                       │
│  (If mutual YES → match! Start real chat)            │
│                                                       │
└──────────────────────┬──────────────────────────────┘
                       ▼
        [Back to Lobby] or [View Matches]
```

### 2.3 The Other Side's Experience

The person whose agent was sent out gets notified:

```
📱 "Your agent went on 3 dates tonight!"
   - Date with Sarah: She voted HOT 🔥 (thought you were real!)
   - Date with Mike: He voted NOT ❌ (knew it was bot)
   - Date with Jess: She voted HOT 🔥 (knew it was bot — still liked it!)

   [Review & decide who to match with →]
```

They then get their own second chance: see the other person's profile + how they reacted, and decide to match or pass.

### 2.4 Match & Chat

Once mutual, both users enter a chat room. The ice is already broken — the system shows a summary:

```
"You matched! Here's how it went down:"
Sarah talked to Alex's bot for 5 min → voted HOT → reveal: BOT → still said YES
Alex's bot had this highlight: [clip of funniest moment]

[Start chatting →] [Schedule a real video date →]
```

---

## 3. Core Features (MVP)

### Must Have (Launch)
- [ ] User auth (email + Google OAuth)
- [ ] Profile creation (photos, basic info)
- [ ] Agent training (video recording, personality quiz)
- [ ] Avatar generation (face + voice clone)
- [ ] Matchmaking queue (basic: age, gender, location preferences)
- [ ] Live video dates (5 min, WebRTC)
- [ ] AI agent video dates (avatar + personality engine)
- [ ] HOT/NOT voting
- [ ] HOT/BOT guessing
- [ ] The Reveal animation
- [ ] Second Chance mechanic
- [ ] Match system (mutual yes → chat)
- [ ] Text chat for matches
- [ ] Basic notifications (email + browser push)
- [ ] Share reveal clips (screenshot/video export)
- [ ] Responsive web (desktop + mobile browsers)

### Nice to Have (V2)
- [ ] Agent replays (watch your agent's dates)
- [ ] Agent training improvements (connect socials, more data)
- [ ] Agent leaderboard ("most convincing agents")
- [ ] Group spectator mode (friends watch your date)
- [ ] Scheduled dates (not just random queue)
- [ ] Premium tiers (more rounds, agent customization)
- [ ] Video date recording (with consent) for sharing
- [ ] PWA / native app wrapper

### Not in MVP
- Phone-based auth / SMS verification
- Video filters / AR effects
- Payment / subscriptions (free for launch, monetize after PMF)
- Moderation AI (manual review + reporting for MVP)

---

## 4. Technical Architecture

### 4.1 High-Level Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         CLIENT (Browser)                         │
│                                                                   │
│  ┌──────────┐  ┌──────────┐  ┌───────────┐  ┌───────────────┐  │
│  │  React    │  │  WebRTC  │  │  WebSocket│  │  Media        │  │
│  │  SPA      │  │  Client  │  │  Client   │  │  Recorder     │  │
│  └────┬─────┘  └────┬─────┘  └─────┬─────┘  └───────┬───────┘  │
│       │              │              │                │           │
└───────┼──────────────┼──────────────┼────────────────┼───────────┘
        │              │              │                │
        ▼              ▼              ▼                ▼
┌───────────────────────────────────────────────────────────────────┐
│                        EDGE / CDN (Cloudflare)                    │
│  Static assets, SSL termination, DDoS protection, WebSocket proxy │
└───────────────────────────────────────────────────────────────────┘
        │              │              │                │
        ▼              ▼              ▼                ▼
┌──────────────┐ ┌─────────────┐ ┌──────────────┐ ┌──────────────┐
│   API        │ │  MEDIA      │ │  REALTIME     │ │  UPLOAD      │
│   SERVER     │ │  SERVER     │ │  SERVER       │ │  SERVICE     │
│              │ │             │ │              │ │              │
│  Auth        │ │  LiveKit    │ │  WebSocket   │ │  S3/R2       │
│  Profiles    │ │  (SFU)     │ │  (game state)│ │  (videos,    │
│  Matching    │ │             │ │  Vote/Reveal │ │   photos)    │
│  Agents      │ │  WebRTC    │ │  Presence    │ │              │
│  Matches     │ │  Routing   │ │  Chat        │ │              │
│              │ │             │ │              │ │              │
│  [Node.js/   │ │  [LiveKit  │ │  [Node.js]   │ │  [Cloudflare │
│   Fastify]   │ │   Cloud]   │ │              │ │   R2]        │
└──────┬───────┘ └──────┬──────┘ └──────┬───────┘ └──────────────┘
       │                │               │
       ▼                ▼               ▼
┌───────────────────────────────────────────────────────────────────┐
│                        AI PIPELINE                                 │
│                                                                     │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌──────────┐ │
│  │  SPEECH-TO- │  │ PERSONALITY │  │  TEXT-TO-    │  │  AVATAR  │ │
│  │  TEXT (STT) │→ │  ENGINE     │→ │  SPEECH     │→ │  ENGINE  │ │
│  │             │  │  (LLM)      │  │  (TTS)      │  │          │ │
│  │  Deepgram   │  │  GPT-4o /   │  │  ElevenLabs │  │  HeyGen  │ │
│  │  (streaming)│  │  Claude     │  │  (streaming)│  │  or      │ │
│  │             │  │             │  │             │  │  Custom   │ │
│  │  ~200ms     │  │  ~400ms     │  │  ~300ms     │  │  ~300ms  │ │
│  └─────────────┘  └─────────────┘  └─────────────┘  └──────────┘ │
│                                                                     │
│  Total pipeline latency target: < 1.5 seconds                      │
│                                                                     │
└───────────────────────────────────────────────────────────────────┘
       │
       ▼
┌───────────────────────────────────────────────────────────────────┐
│                        DATA LAYER                                  │
│                                                                     │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐            │
│  │  PostgreSQL  │  │  Redis       │  │  Vector DB   │            │
│  │  (Supabase)  │  │              │  │  (Pinecone/  │            │
│  │              │  │  Sessions    │  │   pgvector)  │            │
│  │  Users       │  │  Queue       │  │              │            │
│  │  Profiles    │  │  Presence    │  │  Personality │            │
│  │  Matches     │  │  Rate limits │  │  embeddings  │            │
│  │  Dates       │  │  Pub/Sub     │  │  Convo       │            │
│  │  Votes       │  │              │  │  memory      │            │
│  │  Messages    │  │              │  │              │            │
│  └──────────────┘  └──────────────┘  └──────────────┘            │
│                                                                     │
└───────────────────────────────────────────────────────────────────┘
```

### 4.2 Request Flow — Real Human Date

```
User A (browser)                    LiveKit SFU                 User B (browser)
     │                                  │                            │
     │──── WebRTC offer ──────────────→│                            │
     │                                  │←── WebRTC offer ──────────│
     │←── WebRTC answer ──────────────│                            │
     │                                  │──── WebRTC answer ────────→│
     │                                  │                            │
     │◄═══════ Peer-to-peer video/audio via SFU ═══════════════════►│
     │                                  │                            │
     │   [5 min timer managed by Realtime Server via WebSocket]     │
     │                                  │                            │
     │──── vote(HOT, guessed:BOT) ────→ Realtime Server             │
     │                                  │←── vote(NOT, guessed:REAL)─│
     │                                  │                            │
     │←── reveal(was: REAL) ──────────│──── reveal(was: REAL) ─────→│
     │                                  │                            │
     │──── secondChance(YES) ─────────→│                            │
     │                                  │←── secondChance(YES) ─────│
     │                                  │                            │
     │←── MATCH! ─────────────────────│──── MATCH! ────────────────→│
```

### 4.3 Request Flow — AI Agent Date

```
User A (browser)              LiveKit SFU        AI Pipeline         Agent B (server)
     │                            │                   │                     │
     │── WebRTC offer ──────────→│                   │                     │
     │←── WebRTC answer ─────────│                   │                     │
     │                            │                   │                     │
     │══ Audio stream ══════════►│══════════════════►│                     │
     │                            │                   │                     │
     │                            │    ┌──────────────┤                     │
     │                            │    │ STT: "Hey,   │                     │
     │                            │    │ what do you  │                     │
     │                            │    │ do for fun?" │                     │
     │                            │    └──────┬───────┘                     │
     │                            │           ▼                             │
     │                            │    ┌──────────────┐  ┌────────────────┐│
     │                            │    │ LLM: generate│←─│ User B's       ││
     │                            │    │ response as  │  │ personality    ││
     │                            │    │ User B       │  │ profile +      ││
     │                            │    └──────┬───────┘  │ conversation   ││
     │                            │           │          │ memory         ││
     │                            │           ▼          └────────────────┘│
     │                            │    ┌──────────────┐                     │
     │                            │    │ TTS: generate│                     │
     │                            │    │ audio in     │                     │
     │                            │    │ User B's     │                     │
     │                            │    │ cloned voice │                     │
     │                            │    └──────┬───────┘                     │
     │                            │           ▼                             │
     │                            │    ┌──────────────┐                     │
     │                            │    │ Avatar: sync │                     │
     │                            │    │ User B's face│                     │
     │                            │    │ to audio     │                     │
     │                            │    └──────┬───────┘                     │
     │                            │           │                             │
     │◄══ Video+Audio stream ════│◄══════════┘                             │
     │                            │                                         │
     │   [User A sees what looks like User B on a video call]              │
     │   [Latency: ~1.2-1.5s from User A speaking to Agent responding]    │
```

---

## 5. Real-Time Video Pipeline

### 5.1 Media Server: LiveKit

**Why LiveKit:**
- Open-source WebRTC SFU (Selective Forwarding Unit)
- Built-in support for "Agents" — server-side participants that can publish audio/video tracks
- LiveKit Agents SDK (Python) lets you build AI participants that join rooms
- Sub-100ms media routing latency
- Scales horizontally
- Cloud-hosted option (LiveKit Cloud) or self-hosted
- **LiveKit already has an "AI voice assistant" framework** — we extend it to include video

**How it works for AI dates:**
1. User joins a LiveKit room
2. Server-side AI agent joins the same room as a participant
3. AI agent subscribes to user's audio track → feeds to STT
4. AI pipeline generates response → TTS → Avatar video
5. AI agent publishes avatar video + TTS audio as tracks in the room
6. User sees/hears the avatar as if it's another person on a video call

**For real human dates:**
- Standard 1:1 WebRTC call through LiveKit SFU
- Both users are real participants with camera/mic

### 5.2 Avatar Generation Pipeline

**Option A: HeyGen Interactive Avatar (MVP — fastest to market)**
- User uploads 2 selfies (front + side) + 30s voice recording during onboarding
- HeyGen generates a personalized avatar model from photos
- Real-time streaming avatar API: send audio → get video frames
- Latency: ~500ms
- Cost: ~$0.10-0.50/min
- Quality: Very good, passes casual inspection on a video call

**Option B: Custom Pipeline (V2 — better control & cost)**
```
User's reference photo/video
         │
         ▼
┌─────────────────┐      ┌─────────────────┐
│  LivePortrait   │      │  Audio input     │
│  (expression    │◄─────│  (from TTS)      │
│   generation)   │      └─────────────────┘
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Wav2Lip        │
│  (lip sync      │
│   correction)   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Real-ESRGAN    │
│  (face          │
│   enhancement)  │
└────────┬────────┘
         │
         ▼
   Video frames → LiveKit track
```

**For MVP: Use HeyGen. Migrate to custom pipeline when unit economics demand it.**

### 5.3 Voice Pipeline

```
User speaks → Deepgram STT (streaming, ~200ms) →
  Text → LLM (streaming, first token ~300ms) →
    Response text (streaming) → ElevenLabs TTS (streaming, ~300ms) →
      Audio chunks → Avatar engine (frame-by-frame, ~100ms per frame) →
        Video + Audio → LiveKit → User's browser
```

**Key optimization: STREAMING EVERYTHING**

We don't wait for the full STT transcript. We don't wait for the full LLM response. We don't wait for the full TTS audio. Each stage streams its output to the next stage as chunks arrive. This is how we hit <1.5s perceived latency.

```
Time:  0ms    200ms   400ms   600ms   800ms   1000ms  1200ms
       │       │       │       │       │       │       │
STT:   [===listening===][transcript ready]
LLM:                    [==generating==][first tokens]→[streaming...]
TTS:                                    [==first chunk==][streaming...]
Avatar:                                         [==first frame==][streaming...]
User:                                                   [hears first word]
                                                        ↑
                                            ~1.0-1.2s from end of user speech
```

---

## 6. AI Agent System

### 6.1 Personality Engine

Each user's agent has a **personality profile** composed of:

```json
{
  "userId": "abc123",
  "agent": {
    "basePersonality": {
      "humor": "dry, sarcastic, self-deprecating",
      "energy": "chill but engaged",
      "flirtStyle": "witty banter, light teasing",
      "interests": ["climbing", "horror movies", "cooking", "startups"],
      "dealBreakers": ["smoking", "no sense of humor"],
      "values": ["ambition", "kindness", "adventure"],
      "communicationStyle": "asks questions, tells stories, uses callbacks"
    },
    "trainingData": {
      "selfies": ["front_face_url", "side_face_url"],
      "voiceRecording": "30s_audio_url",
      "quizAnswers": {...},
      "voiceProfile": "elevenlabs_voice_id_xyz",
      "avatarModel": "heygen_avatar_id_xyz"
    },
    "conversationMemory": {
      "pastDates": [...],
      "learnedPreferences": [...],
      "successfulTopics": [...],
      "failedTopics": [...]
    },
    "systemPrompt": "You are {name}'s AI dating agent. You're on a 5-minute video date. Your goal is to be authentically {name} — their humor, their vibe, their energy. You are NOT an AI assistant. You are playing the role of {name} on a date. Be natural. Be human. Flirt if it feels right. Ask questions. Tell stories. Be {name}."
  }
}
```

### 6.2 Conversation Management

The LLM doesn't just freestyle — it follows a loose structure:

```
Minute 0-1: Ice breaker (light, fun opener)
Minute 1-3: Discovery (learn about each other, find common ground)
Minute 3-4: Go deeper (share something real, be vulnerable)
Minute 4-5: Close strong (callback humor, leave them wanting more)
```

The agent also has **reactive behaviors:**
- If the user is quiet → ask an engaging question
- If the user is dominating → listen, react, then redirect
- If awkward silence > 5 seconds → deploy a conversation starter
- If the user asks "are you real?" → deflect playfully (this IS the game)

### 6.3 Agent Learning

After each date, the agent improves:
- Which topics got positive reactions (user laughed, engaged more)
- Which responses fell flat (user went quiet, changed subject)
- The agent's "success rate" (how often dates vote HOT)
- Over time, each agent gets better at being their human

---

## 7. Infrastructure & Stack

### 7.1 Frontend
| Component | Technology | Why |
|-----------|-----------|-----|
| Framework | **Next.js 14** (App Router) | SSR for SEO/landing, CSR for app, API routes |
| Styling | **Tailwind CSS** | Fast iteration, responsive |
| State | **Zustand** | Lightweight, good for real-time state |
| Video | **LiveKit Client SDK** | WebRTC abstraction, handles connectivity |
| WebSocket | **Native WebSocket** | Game state, votes, reveals |
| Animations | **Framer Motion** | Reveal animations, transitions |
| Hosting | **Vercel** | Edge functions, fast deploys, preview URLs |

### 7.2 Backend
| Component | Technology | Why |
|-----------|-----------|-----|
| API Server | **Node.js + Fastify** | Fast, TypeScript, lightweight |
| Auth | **Supabase Auth** | Email, Google, Apple — zero custom auth code |
| Database | **Supabase (PostgreSQL)** | Amanda already has account, pgvector for embeddings |
| Cache/Queue | **Upstash Redis** | Serverless Redis, matchmaking queue, rate limits |
| Real-time | **Supabase Realtime** or **WebSocket on Fastify** | Vote syncing, match notifications |
| Media Server | **LiveKit Cloud** | Managed WebRTC SFU |
| File Storage | **Cloudflare R2** or **Supabase Storage** | Photos, training videos, reveal clips |
| Hosting | **Railway** or **Fly.io** | Containerized backend, global regions |

### 7.3 AI Pipeline
| Component | Technology | Why |
|-----------|-----------|-----|
| STT | **Deepgram** (streaming) | Fastest streaming STT, ~200ms |
| LLM | **GPT-4o** (streaming) | Best conversational quality, fast |
| TTS | **ElevenLabs** (streaming) | Best voice cloning, real-time API |
| Avatar | **HeyGen Interactive Avatar** (MVP) | Fastest to production-quality avatar |
| Agent Framework | **LiveKit Agents SDK** (Python) | Built for exactly this use case |
| Personality | **pgvector** in Supabase | RAG for personality context retrieval |

### 7.4 DevOps
| Component | Technology |
|-----------|-----------|
| CI/CD | GitHub Actions |
| Monitoring | Sentry (errors) + Grafana (metrics) |
| Logging | Axiom or Logtail |
| DNS/CDN | Cloudflare |
| Secrets | Doppler or Vercel env vars |

---

## 8. Data Model

```sql
-- Users
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email TEXT UNIQUE NOT NULL,
    name TEXT NOT NULL,
    age INT NOT NULL,
    gender TEXT NOT NULL,
    interested_in TEXT[] NOT NULL, -- ['male', 'female', 'nonbinary']
    city TEXT,
    lat FLOAT,
    lng FLOAT,
    photos TEXT[], -- URLs
    bio TEXT,
    created_at TIMESTAMPTZ DEFAULT now(),
    last_active TIMESTAMPTZ DEFAULT now()
);

-- Agent profiles
CREATE TABLE agents (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) ON DELETE CASCADE,
    voice_id TEXT, -- ElevenLabs voice clone ID
    avatar_id TEXT, -- HeyGen avatar ID
    personality JSONB NOT NULL, -- structured personality profile
    system_prompt TEXT, -- generated system prompt
    training_video_url TEXT,
    training_status TEXT DEFAULT 'pending', -- pending, processing, ready, failed
    success_rate FLOAT DEFAULT 0.5,
    total_dates INT DEFAULT 0,
    created_at TIMESTAMPTZ DEFAULT now(),
    updated_at TIMESTAMPTZ DEFAULT now()
);

-- Personality embeddings (for matching + RAG)
CREATE TABLE personality_embeddings (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) ON DELETE CASCADE,
    content TEXT NOT NULL, -- the text chunk
    embedding VECTOR(1536), -- OpenAI embedding
    category TEXT -- 'interest', 'story', 'preference', 'humor'
);

-- Dates (each video encounter)
CREATE TABLE dates (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_a_id UUID REFERENCES users(id), -- the live participant
    user_b_id UUID REFERENCES users(id), -- whose profile/agent was shown
    is_bot BOOLEAN NOT NULL, -- was user_b represented by their agent?
    livekit_room TEXT, -- LiveKit room name
    started_at TIMESTAMPTZ,
    ended_at TIMESTAMPTZ,
    duration_seconds INT,
    status TEXT DEFAULT 'pending' -- pending, active, completed, cancelled
);

-- Votes
CREATE TABLE votes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    date_id UUID REFERENCES dates(id) ON DELETE CASCADE,
    voter_id UUID REFERENCES users(id),
    hot_or_not TEXT NOT NULL, -- 'hot' or 'not'
    guessed_bot BOOLEAN NOT NULL, -- did they guess it was a bot?
    guessed_correctly BOOLEAN, -- computed after reveal
    second_chance BOOLEAN, -- after reveal, do they want to match?
    created_at TIMESTAMPTZ DEFAULT now()
);

-- Matches (mutual second_chance = true)
CREATE TABLE matches (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_a_id UUID REFERENCES users(id),
    user_b_id UUID REFERENCES users(id),
    date_id UUID REFERENCES dates(id),
    created_at TIMESTAMPTZ DEFAULT now(),
    UNIQUE(user_a_id, user_b_id)
);

-- Messages (post-match chat)
CREATE TABLE messages (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    match_id UUID REFERENCES matches(id) ON DELETE CASCADE,
    sender_id UUID REFERENCES users(id),
    content TEXT NOT NULL,
    created_at TIMESTAMPTZ DEFAULT now()
);

-- Agent conversation logs (for learning)
CREATE TABLE agent_conversations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    date_id UUID REFERENCES dates(id) ON DELETE CASCADE,
    agent_user_id UUID REFERENCES users(id), -- whose agent was it
    transcript JSONB, -- [{role, content, timestamp}]
    outcome TEXT, -- 'hot', 'not'
    second_chance BOOLEAN,
    created_at TIMESTAMPTZ DEFAULT now()
);
```

---

## 9. Latency Budget

**Target: User speaks → Agent starts responding in < 1.5 seconds**

| Stage | Target | Technology | Notes |
|-------|--------|-----------|-------|
| Audio capture + network | 50ms | WebRTC via LiveKit | Negligible |
| Speech-to-Text | 200ms | Deepgram streaming | Partial results as user speaks |
| LLM first token | 400ms | GPT-4o streaming | Use conversation context window |
| TTS first audio chunk | 300ms | ElevenLabs streaming | Start as LLM tokens arrive |
| Avatar first frame | 300ms | HeyGen streaming | Start as audio chunks arrive |
| Video encoding + network | 100ms | LiveKit | H.264/VP8 |
| **Total (worst case)** | **~1,350ms** | | **Acceptable for video call feel** |
| **Total (optimized, pipelined)** | **~800-1,000ms** | | **With aggressive streaming** |

**Comparison to natural conversation:**
- Average human response time in conversation: 200-500ms
- Average human response time on video call: 500-1,500ms (network delay)
- Our target: 800-1,350ms → **Feels like a normal video call with slight lag**

---

## 10. Cost Model

### Per-Date Costs (5-minute AI date)

| Service | Per-Minute Cost | 5-Min Date |
|---------|----------------|-----------|
| Deepgram STT | $0.0043/min | $0.02 |
| GPT-4o (est. 2K tokens/min) | ~$0.02/min | $0.10 |
| ElevenLabs TTS | $0.018/min | $0.09 |
| HeyGen Avatar | ~$0.10-0.50/min | $0.50-2.50 |
| LiveKit Cloud | $0.004/min | $0.02 |
| **Total per AI date** | | **$0.73-2.73** |

### Per-Date Costs (5-minute Real date)

| Service | 5-Min Date |
|---------|-----------|
| LiveKit Cloud | $0.02 |
| **Total per real date** | **$0.02** |

### Monthly Cost Projections

| Users | Dates/Day | AI Ratio | Monthly AI Cost | Monthly Infra | Total |
|-------|----------|----------|----------------|--------------|-------|
| 1,000 | 3,000 | 50% | $33K-$123K | $2K | $35K-$125K |
| 10,000 | 30,000 | 50% | $330K-$1.23M | $10K | $340K-$1.24M |

**⚠️ HeyGen is the cost bottleneck.** Moving to custom avatar pipeline in Phase 2 could reduce avatar cost by 80-90%.

**Revenue needed to break even at 10K users:**
- If $1.50/AI date avg → need $675K/mo revenue
- At $12.99/mo × 10K users = $130K/mo → **not enough on subs alone**
- Need to either: reduce AI cost (custom pipeline), limit AI dates, or add higher-priced tiers

**Strategy:** Free tier gets 2 AI dates/day (most dates are human-to-human). Premium gets unlimited. This keeps AI costs manageable while the human dates are nearly free.

---

## 11. Security & Trust

### Identity Verification
- **MVP:** Email verification + photo upload (manual review for obvious fakes)
- **V2:** Selfie verification (take a live photo, compare to uploaded photos)
- **V2:** Age verification via ID scan (Persona or Veriff API)

### Consent & Safety
- Users explicitly consent to having their avatar/voice used during onboarding
- Users can delete their agent at any time (full data deletion)
- All video dates have a **report** button and **end call** button
- No date recordings saved without explicit consent from both parties
- AI agents cannot discuss explicit sexual content
- AI agents have safety rails (cannot be manipulated into harmful content)

### Anti-Abuse
- Rate limiting on account creation
- Photo moderation (nudity detection via AWS Rekognition or similar)
- Agent conversation monitoring for policy violations
- Block/report system with human review queue

### Data Privacy
- All voice/video data encrypted in transit (TLS/DTLS via WebRTC)
- Training videos encrypted at rest
- Voice clones and avatar models tied to user account, deleted on account deletion
- GDPR/CCPA compliant data handling
- No training on user data for general model improvement without consent

---

## 12. MVP Scope & Phases

### Phase 0: Prototype (2-3 weeks)
**Goal:** Working demo of an AI avatar video date
- [ ] HeyGen avatar from a test user's video
- [ ] GPT-4o personality engine with hardcoded personality
- [ ] ElevenLabs voice clone
- [ ] LiveKit room with human + AI agent
- [ ] Basic web UI showing the video call
- **Deliverable:** Video of a human having a 2-min conversation with an AI avatar

### Phase 1: MVP (8-12 weeks)
**Goal:** Playable game loop with real users
- [ ] Landing page + waitlist → auth → onboarding
- [ ] Agent training pipeline (video upload → avatar + voice + personality)
- [ ] Matchmaking queue (basic preferences)
- [ ] Video date flow (real OR AI, system decides)
- [ ] Vote → Reveal → Second Chance flow
- [ ] Match system + basic text chat
- [ ] Responsive web app
- **Deliverable:** Invite 50-100 beta users, they can play the full game

### Phase 2: Polish & Scale (8-12 weeks)
**Goal:** Product-market fit signals
- [ ] Agent replays (watch how your agent did)
- [ ] Agent improvement (learns from dates)
- [ ] Better matchmaking (personality compatibility, not just preferences)
- [ ] Push notifications
- [ ] Reveal clip sharing (shareable video/image)
- [ ] Payment + premium tiers
- [ ] Custom avatar pipeline (replace HeyGen, cut costs)
- [ ] Moderation system
- **Deliverable:** Public launch, 1,000+ users

### Phase 3: Growth (ongoing)
- [ ] Mobile app (React Native wrapper or native)
- [ ] Agent leaderboard + gamification
- [ ] Group/spectator mode
- [ ] Brand partnerships
- [ ] Content creator program
- [ ] International expansion

---

## 13. Open Questions

1. **Agent vs Real ratio** — What % of dates should be AI? 50/50? Random? Should users know the overall odds?

2. **Can you opt out of bot dates?** If yes, the game breaks. If no, some users will be frustrated. Maybe premium users can see a "likelihood score" after voting?

3. **What happens when BOTH sides are bots?** Two AI agents talking to each other — is that a feature (training/entertainment) or a bug?

4. **Scheduling vs spontaneous** — Is this "open the app and play now" or "schedule a date for 8pm"? Spontaneous needs enough concurrent users. Scheduled guarantees matches but feels less exciting.

5. **How do we seed the initial user base?** Need enough users in each city for real-time matching. Possible approach: launch city-by-city with events, or go global and accept higher % of AI dates initially.

6. **Legal:** Using someone's likeness for an AI avatar in a dating context — what are the legal implications? Need to consult with IP/privacy attorney.

7. **What if the AI is better than the person?** If users consistently prefer the bot version... is that a product problem or a feature? Could lead to "AI dating coach" upsell.

8. **Voice-only mode as fallback?** If avatar generation is too expensive/slow, voice-only dates (with avatar photo displayed) could be a compelling cheaper format. Basically Love Is Blind in your browser.

---

## Appendix: Competitive Landscape

| Product | What | How We're Different |
|---------|------|-------------------|
| Hinge/Bumble/Tinder | Swipe dating | We're a game, not a chore. AI does the hard part. |
| Replika | AI companion | We connect real humans. The AI is the bridge, not the destination. |
| Character.ai | AI personas | Entertainment, not dating. No real human connection. |
| HeyGen | Avatar video | Tool, not product. We're the dating experience built on top. |
| Omegle (RIP) | Random video chat | We add AI, game mechanics, and actual matching. |
| Love Is Blind (show) | Personality-first dating | We're this — but as an app, with AI, and you play every night. |

---

*"It's a Turing test you want to fail."*
