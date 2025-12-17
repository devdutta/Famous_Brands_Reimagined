# Food Brands Reimagined - Google Slides Content

> Copy each slide's content into Google Slides. Suggested theme: Dark or Modern.

---

## SLIDE 1: Title Slide

**Title:**
```
Food Brands Reimagined
```

**Subtitle:**
```
Automated AI Video Generation with n8n
```

**Bottom text:**
```
n8n Automation Workshop | [Your Name] | [Date]
```

**Background suggestion:** Dark gradient with subtle brand icons or miniature building image

---

## SLIDE 2: What We're Building

**Title:**
```
What We're Building
```

**Content (use large centered text):**
```
Transform brand names into stunning AI-generated videos
featuring miniature architectural worlds
```

**Below - Add the sample video thumbnail/GIF:**
```
▶ Watch Sample: https://f002.backblazeb2.com/file/creatomate-c8xg3hsxdu/59de5667-910e-4105-8956-67b1e9754470.mp4
```

**Speaker Notes:**
- Show the sample video (20 seconds)
- Point out: 4 brands, unique buildings, custom music
- "This entire video was generated automatically by n8n"

---

## SLIDE 3: Use Cases - Primary

**Title:**
```
Use Cases
```

**Content (use icon + text layout):**

| Icon | Use Case |
|------|----------|
| 📱 | **Social Media Content** - Instagram Reels, TikTok, YouTube Shorts |
| 🏢 | **Brand Awareness** - Creative brand representations that stand out |
| 🎨 | **Marketing Agencies** - Rapid prototyping of concepts for clients |
| 🍔 | **Food & Restaurant Brands** - Already configured! |

**Speaker Notes:**
- The 9:16 vertical format is perfect for social media
- Marketing teams can generate dozens of concepts quickly
- Current workflow is food-focused but easily adaptable

---

## SLIDE 4: Use Cases - Adaptable

**Title:**
```
Adaptable for Any Industry
```

**Content (use a 2x3 grid):**

```
┌─────────────────────┬─────────────────────┐
│  🛍️ Product         │  🎓 Educational     │
│  Showcases          │  Content            │
├─────────────────────┼─────────────────────┤
│  📹 UGC-Style       │  🏠 Real Estate     │
│  Videos             │  Marketing          │
├─────────────────────┼─────────────────────┤
│  🎉 Event           │  🎮 Gaming &        │
│  Promotion          │  Entertainment      │
└─────────────────────┴─────────────────────┘
```

**Bottom text:**
```
Just modify the system prompt to change the creative direction
```

**Speaker Notes:**
- The modular design means swapping use cases is easy
- Only the AI prompt needs to change - the pipeline stays the same
- Show how flexible the architecture is

---

## SLIDE 5: The Pipeline - Overview

**Title:**
```
Workflow Pipeline
```

**Content (create a horizontal flow diagram):**

```
📊 Input    →    🤖 AI Prompts    →    🎨 Generate    →    🎬 Output
                                         Media

Google        GPT-4.1 creates       Images, Videos,     Creatomate
Sheets        image/video/audio     Audio generated     combines all
              prompts               in parallel         into final video
```

**Speaker Notes:**
- 4 main phases: Input → Prompt → Generate → Output
- The beauty is parallelization - all 4 brands process simultaneously
- Total time: ~10-15 minutes per batch of 4 brands

---

## SLIDE 6: Architecture Diagram

**Title:**
```
Workflow Architecture
```

**Content (create this as an infographic - use shapes and arrows):**

```
┌──────────────────────────────────────────────────────────────────┐
│                         DATA INPUT                                │
│  ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐       │
│  │Schedule │ → │ Google  │ → │  Set    │ → │  Split  │        │
│  │Trigger  │    │ Sheets  │    │ Fields  │    │  Out    │        │
│  └─────────┘    └─────────┘    └─────────┘    └─────────┘       │
└──────────────────────────────────────────────────────────────────┘
                                    ↓
┌──────────────────────────────────────────────────────────────────┐
│                      PROMPT GENERATION                            │
│           ┌─────────┐         ┌─────────────────┐                │
│           │ GPT-4.1 │    →    │ Prompt Generator │               │
│           └─────────┘         │ (AI Agent)       │               │
│                               └─────────────────┘                │
└──────────────────────────────────────────────────────────────────┘
                                    ↓
┌──────────────────────────────────────────────────────────────────┐
│                      MEDIA GENERATION                             │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐          │
│  │   Image     │    │   Video     │    │   Audio     │          │
│  │  (Fal.ai)   │ → │  (Kling)    │    │ (ElevenLabs)│          │
│  └─────────────┘    └─────────────┘    └─────────────┘          │
└──────────────────────────────────────────────────────────────────┘
                                    ↓
┌──────────────────────────────────────────────────────────────────┐
│                      FINAL OUTPUT                                 │
│       ┌─────────────┐         ┌─────────────────┐                │
│       │  Creatomate │    →    │  Google Sheets  │               │
│       │   Render    │         │    (Update)     │               │
│       └─────────────┘         └─────────────────┘                │
└──────────────────────────────────────────────────────────────────┘
```

**Speaker Notes:**
- Walk through each section
- Emphasize the polling loops for async API calls
- Point out Google Sheets as both input AND output

---

## SLIDE 7: Node Breakdown - Input Phase

**Title:**
```
Phase 1: Data Input
```

**Content (use numbered list with icons):**

```
1️⃣  Schedule Trigger
    Runs workflow automatically (daily, hourly, etc.)

2️⃣  Google Sheets
    Fetches rows where Status = "To Do"
    Contains: Category + 4 Brand Names

3️⃣  Set Fields
    Converts brand columns into array
    ["Haldiram", "Wow Momo", "BBQ Nation", "Subway"]

4️⃣  Split Out
    Splits array into 4 separate items
    Each brand processes independently
```

**Speaker Notes:**
- Google Sheets is the control center
- Status column prevents re-processing
- Split enables parallel processing

---

## SLIDE 8: Node Breakdown - AI Phase

**Title:**
```
Phase 2: AI Prompt Generation
```

**Content:**

```
🧠  GPT-4.1 + AI Agent Node

For EACH brand, generates 3 coordinated prompts:

┌─────────────────────────────────────────────────────┐
│  📸 IMAGE PROMPT                                    │
│  "A hyperrealistic tiny building shaped like       │
│   [brand's iconic product]..."                     │
├─────────────────────────────────────────────────────┤
│  🎬 VIDEO PROMPT                                    │
│  "A slow 5-second cinematic pan with tiny          │
│   people moving around the building..."            │
├─────────────────────────────────────────────────────┤
│  🎵 AUDIO PROMPT                                    │
│  "A chill, brand-aligned lo-fi beat that           │
│   matches the brand's vibe..."                     │
└─────────────────────────────────────────────────────┘
```

**Key insight:**
```
The System Message is the creative brain of the workflow
```

**Speaker Notes:**
- The system prompt is ~500 words of detailed creative direction
- Includes examples so the AI knows the exact style
- All 3 prompts are designed to work together

---

## SLIDE 9: Node Breakdown - Generation Phase

**Title:**
```
Phase 3: Media Generation
```

**Content (use 3 columns):**

```
     🖼️ IMAGES              🎬 VIDEOS              🎵 AUDIO
    ───────────           ───────────           ───────────

    Fal.ai                Fal.ai                ElevenLabs
    Flux Pro              Kling v2.6            Sound Gen

    ↓                     ↓                     ↓

    Submit request        Submit request        Generate
    Wait 12 sec           Wait 5 min            5-sec clip
    Poll status           Poll status           Upload to
    Get image URL         Get video URL         Google Drive

    ⏱️ ~15 seconds        ⏱️ ~5 minutes         ⏱️ ~10 seconds
```

**Speaker Notes:**
- Image feeds into video (image-to-video generation)
- Polling loops handle async API responses
- Audio generates independently, stored in Drive for public URL

---

## SLIDE 10: Node Breakdown - Output Phase

**Title:**
```
Phase 4: Final Composition
```

**Content:**

```
                    4 Videos + 4 Audio Clips
                              ↓
                    ┌─────────────────┐
                    │    Aggregate    │
                    │   (Combine)     │
                    └────────┬────────┘
                             ↓
                    ┌─────────────────┐
                    │   Creatomate    │  ← Video editing API
                    │     Render      │    Combines all media
                    └────────┬────────┘    using template
                             ↓
                    ┌─────────────────┐
                    │  Final Video    │  ← ~20 sec video
                    │   (MP4 URL)     │    Ready for social!
                    └────────┬────────┘
                             ↓
                    ┌─────────────────┐
                    │  Update Sheet   │  ← Status = "Done"
                    │                 │    + Video URL saved
                    └─────────────────┘
```

**Speaker Notes:**
- Creatomate is like a cloud-based video editor
- Template defines how 4 segments are arranged
- Google Sheet gets updated automatically - full loop closure

---

## SLIDE 11: Services Used

**Title:**
```
Tech Stack
```

**Content (use logo icons if available):**

```
┌────────────────────────────────────────────────────────────────┐
│                                                                 │
│   📊 Google Sheets     🧠 OpenAI GPT-4.1    🖼️ Fal.ai Flux     │
│   Data Input           Prompt Generation    Image Generation   │
│                                                                 │
│   🎬 Fal.ai Kling      🎵 ElevenLabs        📁 Google Drive    │
│   Video Generation     Audio Generation     File Storage       │
│                                                                 │
│   🎥 Creatomate        ⚡ n8n                                   │
│   Video Composition    Workflow Engine                         │
│                                                                 │
└────────────────────────────────────────────────────────────────┘
```

**Cost per video: ~$3.00 - $3.50**

**Speaker Notes:**
- All services have free tiers or trials
- n8n can be self-hosted (free) or cloud
- Fal.ai and Creatomate are pay-per-use

---

## SLIDE 12: Extendable - Social Media

**Title:**
```
Extend: Auto-Publish to Social Media
```

**Content:**

```
                    Final Video
                         ↓
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
    ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐
    │ YouTube  │  │  TikTok  │  │Instagram │  │ Facebook │
    │  Shorts  │  │          │  │  Reels   │  │  Reels   │
    └──────────┘  └──────────┘  └──────────┘  └──────────┘
```

**Bottom text:**
```
Add n8n nodes to automatically upload to all platforms simultaneously
```

**Speaker Notes:**
- The 9:16 format is already perfect for all platforms
- Just add upload nodes after the final video step
- Can add AI-generated captions per platform

---

## SLIDE 13: Get Started

**Title:**
```
Get Started
```

**Content (large, centered):**

```
📂  GitHub Repository

github.com/devdutta/Famous_Brands_Reimagined
```

**What's included:**
```
✅ Complete workflow JSON (ready to import)
✅ Step-by-step setup documentation
✅ Node-by-node explanation
✅ Cost estimates
✅ Sample output video
```

**Speaker Notes:**
- Walk them through the repo structure
- Point out the sanitized workflow JSON
- Mention the examples folder

---

## SLIDE 14: Q&A

**Title:**
```
Questions?
```

**Content (centered):**

```
🔗 Repository: github.com/devdutta/Famous_Brands_Reimagined

📺 Sample Video: [Show QR code or short link]

📧 Contact: [Your email]
```

**Background:** Use the sample video as a looping background if possible

---

# DESIGN TIPS FOR GOOGLE SLIDES

## Recommended Theme Settings

- **Background:** Dark (#1a1a2e or #16213e)
- **Primary text:** White (#ffffff)
- **Accent color:** Cyan (#00d4ff) or Orange (#ff6b35)
- **Font - Titles:** Montserrat Bold or Poppins Bold
- **Font - Body:** Open Sans or Roboto

## For Architecture Diagrams

1. Use Google Slides' **Diagram** feature (Insert → Diagram)
2. Or use **Shapes** with rounded rectangles
3. Color code by phase:
   - Input: Blue
   - AI: Purple
   - Generation: Green
   - Output: Orange

## For Icons

- Use Google Slides' built-in icons (Insert → Special characters → Emoji)
- Or copy from [Flaticon](https://www.flaticon.com) or [Icons8](https://icons8.com)

## Animations (Optional)

- Use "Fade" for text appearance
- Use "Fly in from left/right" for flow diagrams
- Keep transitions simple - "Fade" between slides

---

# TIMING GUIDE (30 minutes)

| Slide | Time | Notes |
|-------|------|-------|
| 1. Title | 1 min | Introduction |
| 2. What We're Building | 2 min | Show video |
| 3-4. Use Cases | 3 min | Quick overview |
| 5. Pipeline Overview | 2 min | High-level flow |
| 6. Architecture | 3 min | Main diagram |
| 7-10. Node Breakdown | 10 min | Phase by phase |
| 11. Tech Stack | 2 min | Services & costs |
| 12. Extendable | 2 min | Social media extension |
| 13. Get Started | 2 min | GitHub walkthrough |
| 14. Q&A | 3 min | Questions |

**Total: ~30 minutes**
