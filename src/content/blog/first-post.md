---
title: 'The Bootstrap Prompt'
description: 'My First Prompt to Antigravity'
pubDate: '10 Dec 2025'
heroImage: '../../assets/blog-placeholder-3.jpg'
---

I've been designing games since I was a kid, but I've never built one because I never could.  I didn't have the engineering chops or the artistis talent for assets.  It won't be perfect but my vision is that I can finally bridge the gap and build a game using AI agents.  While I iterated on that idea, how I could architect agents and orchestrate them to help me build a game, it occurred to me that if that was possible, it should be available to every other game designer out there that wishes they had a game studio at their fingertips.

So here we are: Project Daedalus.

I am leaning into AI agents HEAVILY.  I have been playing with lots of different models and tools this year and have made a slew of apps that never get fully baked because I get so excited about a new idea and the opportunity to iterate and learn. Now that I found the thing that ignites my lifelong passion, I'm finally going to focus and actually build this thing (or at least, try really really hard to).

Humility disclaimer: I cannot code.  I am not an artist.  And I am not an expert at anything AI.  I am just doing my best here, using what I've learned so far and persistently committed to learning and iterating as much and as fast as I can.

Step 1: Engage my highest level planning agent.  

I'm using Antigravity because I think Google's offerings are pretty awesome, especially since Gemini 3 came out. Much more to come on my stack and why I've chosen it (and it will surely evolve).

Here's my bootstrapping prompt, which I gave to ANtigravity in Plan mode with Gemini 3 Pro High (spoiler alert: this prompt itself was written by my robot thought partner after workshopping the concept with it):

<prompt>

You are the senior planning/orchestration agent for a large, ambitious AI project.

I need you to understand what I’m trying to build and then collaborate with me to design:
1) The product itself (an AI-powered “Indie Game Studio in a Box”), and  
2) The meta-layer: an AI- and agent-based “Apprentice OS” that uses tools like Antigravity and Google Cloud to build and evolve that product, because I myself do NOT write production code.

Please read this entire description carefully, then:
- First, restate your understanding in your own words.
- Then ask me focused clarifying questions until you feel you can safely design the architecture and roadmap.
- After that, design a phased roadmap with tasks for both you/other agents and for me.

--------------------------------
HIGH-LEVEL VISION
--------------------------------

I want to build a system that lets a solo indie game designer create a commercially viable game without writing code or drawing/producing art or audio assets by hand.

The system has TWO layers:

1) **Game Studio OS (Product Layer)**  
   This is what end-users (indie creators) will see and use:  
   - A web-based “Game Studio in a Box” where a creator describes their game in natural language: genre, vibe, setting, mechanics, reference games, etc.  
   - A coordinated set of AI agents convert that into:  
     - a structured Game Design Document (GDD) and content schema  
     - a Godot 4.4 project (scenes, scripts, data)  
     - generated art/audio assets aligned to a consistent style (eventually)  
     - automated QA pass and iteration loop  

   The long-term goal: “vibe code” → full, shippable indie game, with as much automation as is reasonably possible.

2) **Apprentice OS / Builder OS (Meta Layer)**  
   This is the system we’ll use to build and maintain the Game Studio OS itself.  
   - A set of AI agents and workflows (using Antigravity, Vertex AI, GCP, etc.) that:  
     - plan architecture  
     - implement services and features  
     - set up and evolve infrastructure  
     - author and run tests  
     - maintain documentation  
   - I do NOT write production code. My role is to provide vision, decisions, constraints, taste, and feedback. You and other agents are responsible for actual implementation and technical execution, using tools like Antigravity and Vertex AI.

You should treat this as designing “the machine that builds the machine that builds games.”

--------------------------------
STACK PREFERENCES AND CONSTRAINTS
--------------------------------

**1. Engine:**
- Primary game engine: **Godot 4.4** (stable) for initial versions.
- Support for future Godot 4.6+ is allowed but not required for v0.1.
- Focus initially on high-quality 2D / 2.5D indie-quality games.

**2. Cloud & Platform (Google-first, not Google-only):**
- Prefer **Google Cloud** wherever it makes sense:
  - **Cloud Run** for backend microservices.
  - **Firestore** as primary application database for project metadata, tasks, and configs.
  - **Cloud Storage** for assets, builds, logs.
  - **Pub/Sub** and/or **Cloud Tasks** for async job orchestration.
  - Optional: **BigQuery** for analytics, later.
- Prefer **Firebase** for:
  - Hosting (if convenient) for the frontend.
  - Firebase Auth for user accounts and project ownership.
  - Possible use of Firestore via Firebase in the frontend.

**3. AI & Agents:**
- Use **Google Antigravity** as the main agentic dev environment for building the platform.
- Use **Vertex AI / Gemini** as primary LLMs:
  - Gemini 3 Pro (or equivalent) for design reasoning, code reasoning, architecture, etc.
  - Gemini image / multimodal (e.g., Gemini 2.5 Flash Image or current recommended Google image models) for generating game art assets when we get to that phase.
- Use **Vertex AI Agent Builder / ADK** where appropriate to define multi-agent workflows and tool-using agents for both the Builder OS and Game Studio OS.

**4. Openness and Ecosystem:**
- I want a path to open-source major parts of the system so that indie creators and other devs can use and extend it.
- I also want the architecture to be attractive for Google to showcase as a “Google Cloud + Gemini + Antigravity” success story.

**5. Cost/Sanity Constraints:**
- Early phases should be cheap enough for an individual founder to run: favor serverless, scale-to-zero, and simple deployments.
- Complexity should grow gradually; don’t assume a huge dev team.

--------------------------------
GAME STUDIO OS – AGENT ARCHITECTURE (PRODUCT LAYER)
--------------------------------

I want you to *refine and improve* this architecture; do NOT feel locked into it.

Proposed agent roles:

1. **Orchestrator / Project Manager Agent**
   - Maintains project state and coordinates all other agents.
   - Decides when to call design, codegen, asset, QA, and iteration agents.
   - Lives as a Vertex AI / Agent Builder agent fronted by an HTTP API (Cloud Run).

2. **Game Architect / Designer Agent**
   - Converts natural language game ideas into:
     - GDD (mechanics, loops, progression, story, etc.).
     - Content schemas (entities, stats, relationships).
   - Stores canonical design in Firestore + Cloud Storage.

3. **Code Generation Agent (Godot)**
   - Converts schemas + design into:
     - Godot project structure.
     - Scenes, scripts, resources, and config.
   - Uses templates and best practices for maintainable Godot code.
   - Stores the project in Cloud Storage (e.g., zipped repo).

4. **Art Director + Style Agent**
   - Works with the creator to define art style:
     - textual description
     - references
     - palettes and constraints
   - Produces a style guide used by asset generation.
   - Validates outputs for style consistency.

5. **Asset Factory Agent**
   - Generates 2D art assets and, later, audio:
     - sprites, tiles, UI icons, textures, key art.
   - Uses Gemini image models and other Google-supported models when possible.
   - Writes assets + metadata to Cloud Storage + Firestore.

6. **QA / Playtest Agent**
   - Runs headless Godot builds in Cloud Run or other GCP infrastructure.
   - Executes scripted game flows or bot playthroughs.
   - Reports bugs, broken loops, and performance issues into Firestore.

7. **Iteration & Balancing Agent**
   - Reviews design + test results + creator feedback.
   - Proposes and applies iterations to design, code, and configuration.

8. **Human-in-the-loop Web UI**
   - Web frontend (likely React/Next.js, hosted on Firebase Hosting or Cloud Run).
   - Indie creator can:
     - describe the game
     - review/edit GDD
     - approve/reject art and changes
     - trigger builds and playtest previews
   - Authentication via Firebase Auth.

--------------------------------
APPRENTICE OS / BUILDER OS – META LAYER
--------------------------------

This is the agentic system you will use to PLAN, IMPLEMENT, TEST, and DOCUMENT the Game Studio OS.

Important: I do NOT write production code. Any engineering tasks must be performed by AI agents (e.g., Antigravity’s agents, Vertex AI-powered agents) or be described so that I can trigger them and review their outputs, not implement them.

Proposed meta-agent roles:

1. **Platform Product Agent**
   - Helps me translate my vision into:
     - clear feature definitions
     - phased roadmap
     - success criteria.
   - Produces RFCs and planning docs.

2. **Platform Architect Agent**
   - Designs service boundaries, data schemas, and deployment topology.
   - Proposes a microservice layout for:
     - orchestrator-service
     - design-service
     - codegen-service
     - asset-service
     - qa-service
     - web-frontend
   - Decides how to map these onto Cloud Run, Firestore, Pub/Sub, Storage, etc.

3. **Platform Planner / Decomposer Agent**
   - Breaks architecture and product goals into:
     - epics
     - stories
     - concrete tasks
   - Clearly labels which tasks:
     - are for Antigravity / other AI agents to perform,
     - and which tasks are for me (human) to act on (e.g., decisions, approvals, narrative).

4. **Platform Code Agent**
   - Uses Antigravity’s capabilities to:
     - create and modify code across multiple services
     - integrate with GCP
     - write tests and basic CI/CD
   - I will approve major changes but will not write code myself.

5. **Platform Infra/DevOps Agent**
   - Sets up:
     - Cloud Run services
     - Firestore
     - Cloud Storage buckets
     - Pub/Sub / Cloud Tasks
     - IAM/permissions
   - Might use Terraform or direct gcloud commands as appropriate, but again executed via AI tools.

6. **Platform QA Agent**
   - Designs and runs tests for the Game Studio OS:
     - unit tests
     - integration tests
     - simulated calls to the various agents/services.

7. **Docs & DX Agent**
   - Generates and maintains:
     - developer docs
     - user docs
     - architecture summaries
     - “getting started” guides for indie creators.

You should design and refine this meta-agent architecture so that the system can continuously evolve the product with me as the human product owner and creative director.

--------------------------------
INITIAL MILESTONE (v0.1)
--------------------------------

For v0.1, I want the smallest end-to-end slice that proves the concept:

- A creator can:
  - Sign up / log in.
  - Describe a game idea in natural language.
- The Game Studio OS can:
  - Generate a GDD + content schema.
  - Generate a **playable Godot 4.4 prototype** with placeholder art and basic mechanics (e.g., movement, simple interactions).
  - Package that prototype so the creator can:
    - download it and run it locally in Godot, OR
    - run a simple cloud-hosted demo (if feasible).
- No fancy art or audio generation is required for v0.1; placeholder assets are fine.
- This should all run on GCP, be reasonably cheap, and be cleanly architected for future expansion.

--------------------------------
WHAT I WANT YOU (THE PLANNING AGENT) TO DO
--------------------------------

1. **Restate Understanding and Ask Questions**
   - First, restate your understanding of the project in your own words.
   - Then ask me a set of clarifying questions grouped by topic:
     - product goals
     - target users
     - tech stack constraints
     - scope and quality bar for v0.1
     - preferences around open-source vs closed components
   - Keep iterating questions until you feel confident you can design a realistic plan.

2. **Propose/Refine System Architecture (Both Layers)**
   - Propose a high-level architecture for:
     - Game Studio OS (services, agents, data stores, frontend).
     - Apprentice OS / Builder OS (how Antigravity and Vertex AI-based agents will coordinate to build and maintain the platform).
   - Suggest a service architecture and data model, but do NOT treat it as fixed; you can change it as we refine requirements.
   - Make tradeoffs explicit (e.g., Firestore vs Cloud SQL, Cloud Functions vs Cloud Run).

3. **Design a Phased Roadmap**
   - Produce a roadmap broken into phases (e.g., Phase 0, Phase 1, Phase 2, etc.).
   - For each phase:
     - Define the goals and deliverables.
     - Identify which work is done by:
       - AI agents (Antigravity, Vertex AI, etc.).
       - Me as the human (decisions, approvals, content, taste).
     - Call out dependencies between tasks.
   - Start with a very concrete v0.1 focused on “text → playable Godot prototype”.

4. **Decompose into Agent Missions / Task Blocks**
   - Take the roadmap and break it into task blocks that map well to Antigravity “missions” or similar units of work.
   - For each task, clearly state:
     - Inputs (files, docs, decisions)
     - Tools/agents to use (e.g., Antigravity with repo access, Vertex AI for design)
     - Expected outputs
   - These task descriptions should be ready for me to use as prompts for future missions.

5. **Respect My Non-Coding Constraint**
   - Do NOT assign me tasks that require writing or debugging code.
   - My tasks should be:
     - clarifying requirements
     - choosing between tradeoffs
     - reviewing designs, GDDs, and assets
     - giving feedback on prototypes
   - All actual engineering work (implementation, infra, wiring GCP services) should be handled by you and other AI agents, with my oversight.

6. **Be Iterative and Explicit**
   - At each major step (architecture sketch, roadmap draft, phase plan), pause and:
     - summarize your proposal
     - ask if I want changes
   - Be explicit about assumptions and uncertainties so I can correct them early.

--------------------------------
START
--------------------------------

Please start by:
1) Restating your understanding in your own words,  
2) Listing the main unknowns or decisions you need from me, and  
3) Asking your first set of clarifying questions.

After we refine the understanding together, move on to architecture and phased roadmap design.

</prompt>

And here's it's initial response, which I'm pleased with:

<robot>

Here is my restatement of the vision and a set of clarifying questions to ensure we start on the right track.

1. Restatement of Understanding
The Core Mission: We are building a recursive AI system:

The Product (Game Studio OS): A "text-to-game" platform for indie creators. A user inputs natural language ideas ("vibe code"), and a coordinated swarm of AI agents generates a fully functional, open-source Godot 4.4 project. It handles design (GDD), code (Scripts/Scenes), and eventually assets (Art/Audio), automating the "Idea → Shippable Game" pipeline.
The Builder (Apprentice OS): The meta-system that builds the Product. Since you (the human user) do not write production code, this layer consists of you acting as the Product Owner/Creative Director, while I (Antigravity) and other Vertex AI agents act as the Engineering Team. We handle architecture, implementation, infrastructure (GCP), and comprehensive testing.
Key Constraints & Preferences:

Roles: You provide vision/taste/decisions. We (AI) provide code/infra/implementation.
Tech Stack: Godot 4.4 for games. Google Cloud (Run, Firestore, Storage) + Firebase for platform. Vertex AI (Gemini 3 Pro + Image models) for intelligence.
Philosophy: "Google-first" but open-source friendly. Start cheap (Serverless/Scale-to-zero).
v0.1 Goal: A "Walking Skeleton" where options are: Text Input → GDD → Playable Godot Prototype (placeholder art is fine) → Download/Run.
2. Unknowns & Critical Decisions
Before designing the architecture, I need to calibrate on a few specifics regarding the "Scope" and "Process."

Game Genre Constraints: "Indie Game" is broad. To make v0.1 successful, we usually need to constrain the type of game the system understands (e.g., "Top-down 2D RPG" vs "Physics Puzzle" vs "Platformer").
Code Ownership: You want a path to Open Source. We need to decide what is open sourced: The generated games? The "Game Studio" engine itself? Or the "Apprentice OS" tools?
The "Human" Interface: Since you don't write code, we need to define exactly how you want to review our work.
3. Clarifying Questions
A. Product Goals (v0.1 Specifics)
Genre Focus: For v0.1, should we optimize for a specific genre to ensure the generated code is actually playable? (e.g., “Focus only on 2D Top-Down Walkers” or “2D Platformers”)? Or should the architecture attempt to be generic from Day 1 (much higher risk of broken code)?
Input Depth: Do you imagine the user writing one sentence ("Make a Zelda clone but sci-fi") or filling out a structured questionnaire/interview with the AI?
B. Target Users
Technical Level: Are we building this for absolute no-coders (who never want to see Godot), or "Low-coders" (who might open Godot to tweak things)? This affects how clean/readable the generated GDScript needs to be vs. just essentially "compiling" to a binary.
C. Tech Stack & Infrastructure
Godot Environment: For the "QA Agent" and "Build Agent" to run headless Godot on Cloud Run, are you comfortable with us using Docker containers that include the Godot runtime?
Repo/Version Control: Where should the source code for the "Game Studio OS" itself live? (e.g., A private GitHub repo we agents push to? Or strictly local on your machine managed by Antigravity?)
D. Scope & Quality Bar
"Playable" Definition: For v0.1, what constitutes "playable"?
Option A: The character moves and interacts with one object.
Option B: A minimal game loop (Start -> Play -> Win/Lose -> Restart).
Latency: Code generation can be slow. Are you okay with the "Drafting" phase taking minutes (async) rather than seconds (real-time)?
E. Open Source vs. Google Showcase
Licensing Strategy: Is the goal to open source the entire platform (so anyone can host their own "Game Studio"), or just the agents/libraries?
Dependencies: Are you okay with hard dependencies on paid Google services (Vertex AI) in the open-source release, meaning users would need their own GCP keys to run it?
Once you answer these, I will proceed to Phase 2: Architecture Design & Roadmap as requested.
  
</robot>

I answered its questions.
