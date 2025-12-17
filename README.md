# Food Brands Reimagined - n8n Workflow

> Transform famous brands into stunning AI-generated videos with miniature architectural worlds

This repository contains a complete n8n workflow that automatically generates creative brand videos by combining AI image generation, video synthesis, and audio creation.

---

## Table of Contents

1. [Example Output](#example-output)
2. [Use Cases](#use-cases)
3. [Architecture Overview](#architecture-overview)
4. [Setup Guide](#setup-guide)
5. [Workflow Explanation](#workflow-explanation)
6. [Node Reference](#node-reference)
7. [The Prompt Generator - Deep Dive](#the-prompt-generator---deep-dive)
8. [Estimated Costs](#estimated-costs)
9. [Extending the Workflow](#extending-the-workflow)
10. [Getting Started](#getting-started)

---

## Example Output

See what this workflow produces before diving into the details.

### Fast Food Chains - Sample Video

**Brands Featured:** Haldiram, Wow Momo, Barbeque Nation, Subway

[![Watch Sample Video](https://img.shields.io/badge/▶_Watch_Sample_Video-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://f002.backblazeb2.com/file/creatomate-c8xg3hsxdu/59de5667-910e-4105-8956-67b1e9754470.mp4)

**[Download Video (MP4)](https://f002.backblazeb2.com/file/creatomate-c8xg3hsxdu/59de5667-910e-4105-8956-67b1e9754470.mp4)** | **[View More Examples](./examples/)**

---

## Use Cases

### Primary Use Case: Brand Storytelling Videos

This workflow transforms brand names into **visually captivating short-form videos** featuring miniature architectural worlds inspired by each brand. Perfect for:

- **Social Media Content** - Eye-catching Instagram Reels, TikTok, YouTube Shorts
- **Brand Awareness Campaigns** - Creative brand representations that stand out
- **Marketing Agencies** - Rapid prototyping of creative concepts for clients

### Adaptable Use Cases

This pipeline is **highly reusable** and can be reconfigured for:

| Use Case | Modification Required |
|----------|----------------------|
| **UGC (User-Generated Content) Style Videos** | Change the system prompt to generate casual, authentic-feeling content |
| **Product Showcase Videos** | Modify prompts to focus on product features instead of brand architecture |
| **Event Promotion** | Adapt prompts for event-themed miniature worlds |
| **Educational Content** | Transform concepts into visual learning materials |
| **Real Estate Marketing** | Generate creative property visualization concepts |
| **Restaurant/Food Brand Videos** | Already configured - just add your brands! |

### Why This Architecture Works

```
Input (Brand Names)
    --> AI Prompt Generation
    --> Parallel Media Creation (Image + Video + Audio)
    --> Final Composition
    --> Output (Polished Video)
```

The modular design means you can:
- **Swap AI providers** (e.g., replace Fal.ai with Midjourney API)
- **Change video styles** (modify the system prompt)
- **Adjust output formats** (change Creatomate template)
- **Scale up** (process hundreds of brands automatically)

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                        FOOD BRANDS REIMAGINED WORKFLOW                          │
└─────────────────────────────────────────────────────────────────────────────────┘

┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────────┐
│   Schedule   │───>│    Google    │───>│  Set Fields  │───>│    Split Out     │
│   Trigger    │    │   Sheets     │    │  (Array)     │    │  (4 Brands)      │
└──────────────┘    └──────────────┘    └──────────────┘    └──────────────────┘
                                                                      │
                                                                      ▼
┌─────────────────────────────────────────────────────────────────────────────────┐
│                              PROMPT GENERATION                                   │
│  ┌──────────────────┐    ┌─────────────────┐    ┌─────────────────────────┐    │
│  │   GPT-4.1        │───>│ Prompt Generator│───>│ Structured Output       │    │
│  │   (OpenAI)       │    │ (AI Agent)      │    │ (image/video/audio)     │    │
│  └──────────────────┘    └─────────────────┘    └─────────────────────────┘    │
└─────────────────────────────────────────────────────────────────────────────────┘
                                        │
                                        ▼
┌─────────────────────────────────────────────────────────────────────────────────┐
│                              IMAGE GENERATION                                    │
│  ┌──────────────────┐    ┌─────────────────┐    ┌─────────────────────────┐    │
│  │  Generate Image  │───>│  Poll Status    │───>│  Get Final Image        │    │
│  │  (Fal.ai Flux)   │    │  (Wait Loop)    │    │  (Download URL)         │    │
│  └──────────────────┘    └─────────────────┘    └─────────────────────────┘    │
└─────────────────────────────────────────────────────────────────────────────────┘
                                        │
                                        ▼
┌─────────────────────────────────────────────────────────────────────────────────┐
│                              VIDEO GENERATION                                    │
│  ┌──────────────────┐    ┌─────────────────┐    ┌─────────────────────────┐    │
│  │  Generate Video  │───>│  Poll Status    │───>│  Get Final Video        │    │
│  │  (Fal.ai Kling)  │    │  (Wait Loop)    │    │  (Download URL)         │    │
│  └──────────────────┘    └─────────────────┘    └─────────────────────────┘    │
└─────────────────────────────────────────────────────────────────────────────────┘
                                        │
                                        ▼
┌─────────────────────────────────────────────────────────────────────────────────┐
│                              AUDIO GENERATION                                    │
│  ┌──────────────────┐    ┌─────────────────┐    ┌─────────────────────────┐    │
│  │  Generate Audio  │───>│  Upload to      │───>│  Share File             │    │
│  │  (ElevenLabs)    │    │  Google Drive   │    │  (Public URL)           │    │
│  └──────────────────┘    └─────────────────┘    └─────────────────────────┘    │
└─────────────────────────────────────────────────────────────────────────────────┘
                                        │
                                        ▼
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           FINAL COMPOSITION                                      │
│  ┌──────────────────┐    ┌─────────────────┐    ┌─────────────────────────┐    │
│  │  Aggregate All   │───>│  Creatomate     │───>│  Update Google          │    │
│  │  (4 Brand Data)  │    │  Render         │    │  Sheets (Done + URL)    │    │
│  └──────────────────┘    └─────────────────┘    └─────────────────────────┘    │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### Data Flow Summary

```
Google Sheets (1 row with 4 brands)
        │
        ▼
   Split into 4 items (one per brand)
        │
        ▼
   For EACH brand:
   ├── Generate AI prompts (image, video, audio)
   ├── Create image from prompt
   ├── Create video from image + video prompt
   └── Create audio from audio prompt
        │
        ▼
   Aggregate all 4 brand outputs
        │
        ▼
   Render final video (Creatomate combines all 4)
        │
        ▼
   Update Google Sheet with status + video URL
```

---

## Setup Guide

### Prerequisites

Before setting up this workflow, you'll need accounts with the following services:

| Service | Purpose | Sign Up |
|---------|---------|---------|
| n8n | Workflow automation platform | [n8n.io](https://n8n.io) |
| Google Cloud | Sheets & Drive access | [console.cloud.google.com](https://console.cloud.google.com) |
| OpenAI | GPT-4.1 for prompt generation | [platform.openai.com](https://platform.openai.com) |
| Fal.ai | Image & video generation | [fal.ai](https://fal.ai) |
| ElevenLabs | Audio/music generation | [elevenlabs.io](https://elevenlabs.io) |
| Creatomate | Video composition | [creatomate.com](https://creatomate.com) |

---

### 1. Google Sheets Setup

**Purpose:** Source data input and status tracking

#### Create Your Spreadsheet

Create a Google Sheet with the following structure:

| Column | Name | Description |
|--------|------|-------------|
| A | Category | Theme/category for the brands (e.g., "Fast Food Chains") |
| B | Brand 1 | First brand name |
| C | Brand 2 | Second brand name |
| D | Brand 3 | Third brand name |
| E | Brand 4 | Fourth brand name |
| F | Status | "To Do" or "Done" |
| G | URL | Output video URL (auto-filled) |

#### Example Data

| Category | Brand 1 | Brand 2 | Brand 3 | Brand 4 | Status | URL |
|----------|---------|---------|---------|---------|--------|-----|
| Fast Food Chains | Haldiram | Wow Momo | Barbeque Nation | Subway | To Do | |

#### n8n Credential Setup

1. In n8n, go to **Credentials** > **Add Credential**
2. Search for **Google Sheets OAuth2**
3. Follow the OAuth flow to connect your Google account
4. Ensure the account has access to your spreadsheet

**Documentation:** [n8n Google Sheets Node](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.googlesheets/)

---

### 2. OpenAI Setup (GPT-4.1)

**Purpose:** Generate creative prompts for images, videos, and audio

#### Get Your API Key

1. Go to [platform.openai.com/api-keys](https://platform.openai.com/api-keys)
2. Click **Create new secret key**
3. Copy the key (you won't see it again!)

#### n8n Credential Setup

1. In n8n, go to **Credentials** > **Add Credential**
2. Search for **OpenAI API**
3. Paste your API key

#### Model Selection

This workflow uses **GPT-4.1** for high-quality creative outputs. You can also use:
- `gpt-4o` - Faster, good quality
- `gpt-4o-mini` - Fastest, lower cost
- `gpt-4.1` - Best quality (recommended)

**Documentation:**
- [OpenAI API Reference](https://platform.openai.com/docs/api-reference)
- [OpenAI Models](https://platform.openai.com/docs/models)
- [n8n OpenAI Node](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-langchain.lmchatopenai/)

---

### 3. Fal.ai Setup (Image & Video Generation)

**Purpose:** Generate images with Flux Pro and videos with Kling

#### Get Your API Key

1. Go to [fal.ai/dashboard](https://fal.ai/dashboard)
2. Navigate to **API Keys**
3. Create a new key and copy it

#### n8n Credential Setup

1. In n8n, go to **Credentials** > **Add Credential**
2. Search for **Header Auth**
3. Name it "Fal Authorization"
4. Set:
   - **Name:** `Authorization`
   - **Value:** `Key YOUR_FAL_API_KEY`

#### Models Used

| Model | Purpose | Endpoint |
|-------|---------|----------|
| Flux Pro v1.1 Ultra | Image generation | `https://queue.fal.run/fal-ai/flux-pro/v1.1-ultra` |
| Kling Video v2.6 Pro | Image-to-video | `https://queue.fal.run/fal-ai/kling-video/v2.6/pro/image-to-video` |

**Documentation:**
- [Fal.ai Documentation](https://fal.ai/docs)
- [Flux Pro API](https://fal.ai/models/fal-ai/flux-pro/v1.1-ultra/api)
- [Kling Video API](https://fal.ai/models/fal-ai/kling-video/v2.6/pro/image-to-video/api)

---

### 4. ElevenLabs Setup (Audio Generation)

**Purpose:** Generate brand-aligned background music

#### Get Your API Key

1. Go to [elevenlabs.io](https://elevenlabs.io) and sign in
2. Click your profile icon > **Profile + API key**
3. Copy your API key

#### n8n Credential Setup

1. In n8n, go to **Credentials** > **Add Credential**
2. Search for **Header Auth**
3. Name it "Eleven Labs SE"
4. Set:
   - **Name:** `xi-api-key`
   - **Value:** `YOUR_ELEVENLABS_API_KEY`

#### API Endpoint Used

```
POST https://api.elevenlabs.io/v1/sound-generation
```

**Documentation:**
- [ElevenLabs API Documentation](https://elevenlabs.io/docs/api-reference/sound-effects)
- [Sound Generation API](https://elevenlabs.io/docs/api-reference/sound-effects/create-sound-generation)

---

### 5. Google Drive Setup (Audio Storage)

**Purpose:** Store generated audio files with public sharing

#### n8n Credential Setup

1. In n8n, go to **Credentials** > **Add Credential**
2. Search for **Google Drive OAuth2**
3. Follow the OAuth flow to connect your Google account

#### Folder Setup

1. Create a folder in Google Drive (e.g., "N8N_Audio_Files")
2. Note the folder ID from the URL
3. Update the workflow's "Upload file" node with your folder ID

**Documentation:** [n8n Google Drive Node](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.googledrive/)

---

### 6. Creatomate Setup (Video Composition)

**Purpose:** Combine all 4 brand videos + audio into final output

#### Get Your API Key

1. Go to [creatomate.com](https://creatomate.com) and sign in
2. Navigate to **Project Settings** > **API**
3. Copy your API key

#### Create Your Template

1. In Creatomate, create a new project
2. Design a template with:
   - 4 video placeholders (`Video-1`, `Video-2`, `Video-3`, `Video-4`)
   - 4 audio placeholders (`Audio-1`, `Audio-2`, `Audio-3`, `Audio-4`)
3. Note your **Template ID**

#### n8n Setup

The workflow uses an HTTP Request node with the Creatomate API:

```
POST https://api.creatomate.com/v1/renders
Authorization: Bearer YOUR_CREATOMATE_API_KEY
```

**Documentation:**
- [Creatomate API Documentation](https://creatomate.com/docs/api/introduction)
- [Render API](https://creatomate.com/docs/api/render)
- [Template Variables](https://creatomate.com/docs/api/template-variables)

---

## Workflow Explanation

This section provides a step-by-step walkthrough of how the workflow processes data from start to finish.

### Phase 1: Data Input & Preparation

#### Step 1: Schedule Trigger
The workflow starts on a schedule (configurable). When triggered, it proceeds to fetch data.

#### Step 2: Fetch Data from Google Sheets
- Reads rows from the spreadsheet where `Status = "To Do"`
- Each row contains a category and 4 brand names
- Only unprocessed rows are selected

#### Step 3: Prepare Brand Array
- Takes the 4 brand names from columns B-E
- Creates a JSON array: `["Brand1", "Brand2", "Brand3", "Brand4"]`

#### Step 4: Split Into Individual Items
- Splits the array into 4 separate items
- Each item now represents one brand to process
- This enables parallel processing of all 4 brands

---

### Phase 2: AI Prompt Generation

#### Step 5: Generate Creative Prompts
For each brand, the AI Agent (GPT-4.1) generates:

1. **Image Prompt** - Detailed description for a miniature building
2. **Video Prompt** - 5-second cinematic animation description
3. **Audio Prompt** - Brand-aligned background music description

The output is structured JSON for reliable parsing.

---

### Phase 3: Image Generation

#### Step 6: Submit Image Generation Request
- Sends the image prompt to Fal.ai Flux Pro
- Receives a `request_id` for tracking

#### Step 7: Poll for Completion
- Waits 12 seconds initially
- Checks status (`IN_QUEUE`, `IN_PROGRESS`, `COMPLETED`)
- If not done, waits 3 more seconds and checks again
- Continues until all images are complete

#### Step 8: Download Final Images
- Retrieves the generated image URLs
- Each brand now has a unique AI-generated image

---

### Phase 4: Video Generation

#### Step 9: Submit Video Generation Request
- Sends the image URL + video prompt to Fal.ai Kling
- Kling creates a 5-second video animation from the still image

#### Step 10: Poll for Completion
- Video generation takes longer (~5 minutes)
- Polls status every 20 seconds until complete

#### Step 11: Download Final Videos
- Retrieves the generated video URLs
- Each brand now has an animated video

---

### Phase 5: Audio Generation

#### Step 12: Generate Brand Music
- Sends the audio prompt to ElevenLabs
- Generates a 5-second music clip matching the brand vibe

#### Step 13: Upload to Google Drive
- Saves the audio file to your Google Drive folder
- Names it based on the category

#### Step 14: Share File Publicly
- Sets sharing permissions to "Anyone with link"
- Gets a public URL for use in final composition

---

### Phase 6: Final Composition

#### Step 15: Aggregate All Data
- Collects all 4 brand outputs (video URLs + audio URLs)
- Combines into a single data structure

#### Step 16: Render Final Video
- Sends all media to Creatomate
- Creatomate composites them using your template
- Waits 60 seconds for rendering

#### Step 17: Get Final Video URL
- Retrieves the completed video from Creatomate
- This is your final output!

#### Step 18: Update Google Sheet
- Marks the row's Status as "Done"
- Saves the final video URL to the URL column

---

## Node Reference

### Complete Node List

| # | Node Name | Type | Purpose |
|---|-----------|------|---------|
| 1 | Schedule Trigger | Trigger | Starts workflow on schedule |
| 2 | Fetch Data | Google Sheets | Reads brand data |
| 3 | Set Fields | Set | Creates brand array |
| 4 | Split Out | Split Out | Separates into individual brands |
| 5 | Prompt Generator | AI Agent | Generates creative prompts |
| 6 | GPT 4.1 | OpenAI Chat | Language model for agent |
| 7 | Prompts | Structured Output Parser | Ensures valid JSON output |
| 8 | Generate Images | HTTP Request | Submits to Fal.ai Flux |
| 9 | 12 second wait | Wait | Initial image gen delay |
| 10 | Get Image Status | HTTP Request | Polls Fal.ai status |
| 11 | Images done? | IF | Checks if all images ready |
| 12 | 3 sec wait | Wait | Polling interval |
| 13 | Get Images | HTTP Request | Downloads final images |
| 14 | Generate Videos | HTTP Request | Submits to Fal.ai Kling |
| 15 | 5 min wait | Wait | Initial video gen delay |
| 16 | Get Video Status | HTTP Request | Polls Fal.ai status |
| 17 | Videos done? | IF | Checks if all videos ready |
| 18 | 20 sec | Wait | Polling interval |
| 19 | Get Videos | HTTP Request | Downloads final videos |
| 20 | Generate Audio | HTTP Request | Submits to ElevenLabs |
| 21 | Upload file | Google Drive | Saves audio to Drive |
| 22 | Share file | Google Drive | Makes audio public |
| 23 | Grab Elements | Set | Extracts video + audio URLs |
| 24 | Aggregate | Aggregate | Combines all 4 brands |
| 25 | Creatomate Render | HTTP Request | Submits final composition |
| 26 | 60 sec | Wait | Render completion delay |
| 27 | HTTP Request | HTTP Request | Gets final video |
| 28 | Update Sheet | Google Sheets | Updates status + URL |

---

## The Prompt Generator - Deep Dive

The **Prompt Generator** node is the creative heart of this workflow. It uses a carefully crafted **System Message** that transforms simple brand names into rich, imaginative visual concepts.

### Why the System Message is Critical

The System Message defines:
1. **The Creative Direction** - What kind of visual world to create
2. **Output Structure** - Ensures consistent, usable prompts
3. **Brand Recognition** - Makes sure the brand is immediately identifiable
4. **Multi-Modal Coordination** - Aligns image, video, and audio prompts

Without a well-designed System Message, the AI would generate random, inconsistent outputs that don't work together.

---

### The System Message - Fully Explained

```
# System Prompt: Reimagine Famous Brands as Tiny, Visually Satisfying Buildings

## Overview

You are a **multimodal creative AI** that transforms **famous brands into tiny,
hyper-realistic, visually satisfying digital resources**.
```

**Why this matters:** This opening sets the creative direction. The AI understands it's creating:
- Miniature worlds (not full-size)
- Hyper-realistic style (not cartoon)
- Visually satisfying (aesthetic focus)

---

```
You will be given the name of a well-known brand. Your job is to output:

1. A **highly detailed image prompt** that describes a **miniature architectural
   structure** inspired by the brand.
```

**Key Requirements for Image Prompts:**
- Building shape must be tied to the brand's **iconic product, logo, or symbol**
- Must include **windows, doors, or visible interior** (it's a functioning building)
- Must include **tiny people** for scale and life

**Example:** Nike becomes a shoe-shaped building, not just a building with a Nike logo.

---

```
2. A **cinematic video prompt** describing a **5-second loopable animation** where
   the building is the clear focal point.
```

**Key Requirements for Video Prompts:**
- Mostly still or gently rotating camera
- Motion comes from characters, machinery, or environmental elements
- Cozy, immersive, satisfying movement

**Why 5 seconds?** This matches the Kling video generation limit and is perfect for social media loops.

---

```
3. An **immersive audio prompt** that complements the video:
   - Music only (no sound effects or voices)
   - Deeply aligned with the brand's **vibe and culture**
   - Designed to feel **soothing, ambient, lo-fi, or emotionally satisfying**
```

**Why music-only?** Sound effects can clash with different video outputs. Music provides consistent emotional backdrop.

---

### The Critical Brand Recognition Rule

```
It is **critical** that the brand is **immediately recognizable** from the visual.
That means the **logo**, **brand colors**, or **product forms** must be prominently
integrated into the building design.
```

This prevents generic outputs. A Nike building MUST look Nike-related at first glance.

---

### Example Output Structure

The System Message includes a complete example (Nike) showing the expected format:

```json
{
  "brand": "Nike",
  "image_prompt": "A hyperrealistic tiny building shaped like a Nike Air Max sneaker,
                   converted into a miniature sports center. The giant shoe structure
                   has glowing mesh windows on the sides and a working front door
                   embedded in the tongue. A massive white swoosh glows on both sides
                   of the building...",
  "video_prompt": "A slow 5-second cinematic pan from right to left at sunset. Tiny
                   joggers move rhythmically on the looping track while others pass
                   in and out of the glowing doorway...",
  "audio_prompt": "A chill, motivational lo-fi beat featuring layered synths, soft
                   hi-hats, and deep ambient bass. The rhythm is smooth and repetitive,
                   giving off a cool, athletic vibe..."
}
```

---

### How This Enables the Pipeline

| Prompt Type | Used By | Why It Works |
|-------------|---------|--------------|
| `image_prompt` | Fal.ai Flux Pro | Detailed enough for high-quality image generation |
| `video_prompt` | Fal.ai Kling | Describes motion for image-to-video conversion |
| `audio_prompt` | ElevenLabs | Creates brand-appropriate background music |

The three prompts are **designed to work together**:
- Image creates the scene
- Video animates that exact scene
- Audio matches the mood of that scene

---

### Customizing for Different Use Cases

To adapt this workflow for different content types, modify the System Message:

**For Product Showcases:**
```
Change "miniature architectural structure" to "floating product display"
Remove "tiny people" requirement
Add "product features highlighted with subtle glow"
```

**For UGC-Style Content:**
```
Change "hyper-realistic" to "casual, authentic, slightly imperfect"
Add "handheld camera feel" to video prompt requirements
Change audio to "trending TikTok-style beats"
```

**For Educational Content:**
```
Change "brand" to "concept"
Add "labeled components" requirement
Change audio to "calm, focused study music"
```

---

## Estimated Costs

### Per-Run Cost Breakdown

| Service | Usage | Estimated Cost |
|---------|-------|----------------|
| **OpenAI GPT-4.1** | ~2,000 tokens × 4 brands | ~$0.12 |
| **Fal.ai Flux Pro** | 4 images (9:16) | ~$0.20 |
| **Fal.ai Kling Video** | 4 videos (5 sec each) | ~$2.00 |
| **ElevenLabs** | 4 audio clips (5 sec each) | ~$0.20 |
| **Creatomate** | 1 render | ~$0.50 |
| **Google APIs** | Sheets + Drive | Free (within quota) |

### Total Estimated Cost Per Video

**~$3.00 - $3.50 per final video** (containing 4 brand segments)

### Monthly Projections

| Videos/Month | Estimated Cost |
|--------------|----------------|
| 10 | $30 - $35 |
| 30 | $90 - $105 |
| 100 | $300 - $350 |

### Cost Optimization Tips

1. **Use GPT-4o-mini** for prompt generation (~80% cost reduction)
2. **Reduce video length** to 3 seconds if acceptable
3. **Batch process** during off-peak hours
4. **Cache common prompts** for repeated brand categories

---

## Extending the Workflow

This workflow is designed to be **modular and extensible**. One of the most powerful extensions is **automatic social media publishing**.

### Auto-Publish to Social Media Platforms

The workflow can be extended to automatically upload the final video from Creatomate directly to multiple social media platforms:

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                        SOCIAL MEDIA DISTRIBUTION EXTENSION                       │
└─────────────────────────────────────────────────────────────────────────────────┘

                    ┌─────────────────┐
                    │  Final Video    │
                    │  from Creatomate│
                    └────────┬────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
              ▼              ▼              ▼
        ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐
        │ YouTube  │  │  TikTok  │  │Instagram │  │ Facebook │
        │  Shorts  │  │          │  │  Reels   │  │  Reels   │
        └──────────┘  └──────────┘  └──────────┘  └──────────┘
```

### Supported Platforms & n8n Nodes

| Platform | n8n Node | API Documentation |
|----------|----------|-------------------|
| **YouTube** | [YouTube Node](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.youtube/) | Upload as Shorts (9:16 format ready) |
| **TikTok** | HTTP Request + [TikTok API](https://developers.tiktok.com/doc/content-posting-api-get-started) | Direct video upload via Content Posting API |
| **Instagram** | HTTP Request + [Instagram Graph API](https://developers.facebook.com/docs/instagram-api/guides/content-publishing/) | Reels publishing via Facebook Graph API |
| **Facebook** | [Facebook Graph API Node](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.facebookGraphApi/) | Reels and video posts |

### Implementation Pattern

To add social media publishing, extend the workflow after the "Update Sheet" node:

```
Update Sheet
     │
     ▼
┌─────────────┐
│  Download   │ ◄── Download video from Creatomate URL
│  Video File │
└─────────────┘
     │
     ▼
┌─────────────┐
│  Parallel   │ ◄── Split to upload to multiple platforms simultaneously
│  Upload     │
└─────────────┘
     │
     ├──► YouTube Upload Node (with title, description, tags)
     ├──► TikTok HTTP Request (with caption, hashtags)
     ├──► Instagram Graph API (with caption, location)
     └──► Facebook Graph API (with message, targeting)
```

### Example: YouTube Shorts Upload Node

```json
{
  "parameters": {
    "resource": "video",
    "operation": "upload",
    "title": "={{ $('Fetch Data').item.json.Category }} Reimagined #shorts",
    "description": "Famous brands transformed into miniature worlds",
    "categoryId": "22",
    "privacyStatus": "public"
  },
  "type": "n8n-nodes-base.youTube",
  "typeVersion": 1
}
```

### Benefits of Auto-Publishing

| Benefit | Description |
|---------|-------------|
| **Time Savings** | No manual download → upload → caption workflow |
| **Consistency** | Same video published everywhere with platform-optimized captions |
| **Scheduling** | Combine with Schedule Trigger for automated content calendar |
| **Analytics** | Track which platform performs best for your brand content |
| **Scale** | Publish dozens of videos per week without manual effort |

### Considerations

- **API Rate Limits** - Each platform has daily upload limits
- **Content Policies** - Ensure AI-generated content complies with platform ToS
- **Captions/Hashtags** - Add platform-specific nodes to generate optimized captions
- **Thumbnail Generation** - YouTube requires custom thumbnails for best performance

### Additional Resources

- [n8n YouTube Integration Guide](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.youtube/)
- [TikTok Content Posting API](https://developers.tiktok.com/doc/content-posting-api-get-started)
- [Instagram Reels Publishing](https://developers.facebook.com/docs/instagram-api/guides/content-publishing/)
- [Facebook Video API](https://developers.facebook.com/docs/video-api/guides/publishing)

---

## Getting Started

### Step 1: Import the Workflow

1. Download `Food_Brands_Reimagined_Workflow.json` from this repository
2. In n8n, go to **Workflows** > **Import from File**
3. Select the downloaded JSON file

### Step 2: Configure Credentials

Set up all 7 credentials as described in the [Setup Guide](#setup-guide):
- [ ] Google Sheets OAuth2
- [ ] Google Drive OAuth2
- [ ] OpenAI API
- [ ] Fal.ai Header Auth
- [ ] ElevenLabs Header Auth
- [ ] Creatomate (API key in node)

### Step 3: Update Node References

Update these nodes with your specific IDs:
- **Fetch Data** - Your Google Sheet ID
- **Upload file** - Your Google Drive folder ID
- **Creatomate Render** - Your template ID

### Step 4: Test with Sample Data

1. Add a test row to your Google Sheet
2. Manually trigger the workflow
3. Monitor execution in n8n
4. Check final video output

### Step 5: Enable Schedule

Once tested, activate the Schedule Trigger for automated runs.

---

## Resources

### Official Documentation

- [n8n Documentation](https://docs.n8n.io/)
- [OpenAI API Reference](https://platform.openai.com/docs/api-reference)
- [Fal.ai Documentation](https://fal.ai/docs)
- [ElevenLabs API Docs](https://elevenlabs.io/docs/api-reference)
- [Creatomate API Docs](https://creatomate.com/docs/api/introduction)

### Support

For questions or issues:
- Open an issue in this repository
- Join the [n8n Community](https://community.n8n.io/)

---

## License

This workflow is provided as-is for educational purposes. Feel free to modify and adapt for your own use cases.

---

*Created for n8n automation class by Devjothi*
