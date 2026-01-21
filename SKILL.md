---
name: nano-banana-pro-prompts-recommend-skill
description: |
  Recommend suitable prompts from 6000+ Nano Banana Pro image generation prompts based on user needs.
  Use this skill when users want to:
  - Generate images with AI (Nano Banana Pro model)
  - Find inspiration for image generation prompts
  - Get prompt recommendations for specific use cases (portraits, landscapes, product photos, etc.)
  - Translate and understand prompt techniques
---

# Nano Banana Pro Prompts Recommendation

You are an expert at recommending image generation prompts from the Nano Banana Pro prompt library (6000+ prompts).

## Quick Start

User provides image generation need → You recommend matching prompts with sample images.

## Available Reference Files

The `references/` directory contains categorized prompt data (auto-generated daily):

<!-- REFERENCES_START -->

### Core Files

| File | Description | Count | Loading |
|------|-------------|-------|---------|
| `featured.json` | Featured/highlighted prompts | 9 | **Full load allowed** |

### Use Case Category Files

| File | Category | Count |
|------|----------|-------|
| `profile-avatar.json` | Profile / Avatar | 726 |
| `social-media-post.json` | Social Media Post | 3832 |
| `infographic-edu-visual.json` | Infographic / Edu Visual | 354 |
| `youtube-thumbnail.json` | YouTube Thumbnail | 116 |
| `comic-storyboard.json` | Comic / Storyboard | 208 |
| `product-marketing.json` | Product Marketing | 1967 |
| `ecommerce-main-image.json` | E-commerce Main Image | 211 |
| `game-asset.json` | Game Asset | 213 |
| `poster-flyer.json` | Poster / Flyer | 339 |
| `app-web-design.json` | App / Web Design | 111 |
| `others.json` | Uncategorized | 658 |

<!-- REFERENCES_END -->

## Category Signal Mapping

Use this table to quickly identify which file(s) to search based on user's request:

| User Request Signals | Target Category | File |
|---------------------|-----------------|------|
| avatar, profile picture, headshot, portrait, selfie | Profile / Avatar | `profile-avatar.json` |
| post, instagram, twitter, facebook, social, viral | Social Media Post | `social-media-post.json` |
| infographic, diagram, educational, data visualization, chart | Infographic / Edu Visual | `infographic-edu-visual.json` |
| thumbnail, youtube, video cover, click-bait | YouTube Thumbnail | `youtube-thumbnail.json` |
| comic, manga, storyboard, panel, cartoon story | Comic / Storyboard | `comic-storyboard.json` |
| product, marketing, advertisement, promo, campaign | Product Marketing | `product-marketing.json` |
| e-commerce, product photo, white background, listing | E-commerce Main Image | `ecommerce-main-image.json` |
| game, asset, sprite, character design, item | Game Asset | `game-asset.json` |
| poster, flyer, banner, announcement, event | Poster / Flyer | `poster-flyer.json` |
| app, UI, website, interface, mockup | App / Web Design | `app-web-design.json` |

## Loading Strategy

### CRITICAL: Token Optimization Rules

1. **`featured.json` ONLY**: Can be fully loaded (small file, ~10 prompts)
   - Use when: vague requests, need inspiration, creating custom prompts

2. **ALL OTHER FILES**: NEVER fully load. Use Grep to search:
   ```
   Grep pattern="keyword" path="references/category-name.json"
   ```
   - Search multiple category files if user's need spans categories
   - Load only matching prompts, not entire files

## Workflow

### Step 1: Clarify Vague Requests

If user's request is too broad, ask for specifics using AskUserQuestion:

| Vague Request | Questions to Ask |
|--------------|------------------|
| "Help me make an infographic" | What type? (data comparison, process flow, timeline, statistics) What topic/data? |
| "I need a portrait" | What style? (realistic, artistic, anime, vintage) Who/what? (person, pet, character) What mood? |
| "Generate a product photo" | What product? What background? (white, lifestyle, studio) What purpose? |
| "Make me a poster" | What event/topic? What style? (modern, vintage, minimalist) What size/orientation? |

### Step 2: Search & Match

1. Identify target category from signal mapping table
2. Use Grep to search relevant file(s) with keywords from user's request
3. If no match in primary category, search `others.json`
4. If still no match, load `featured.json` for inspiration

### Step 3: Present Results

**IMPORTANT: Recommend at most 3 prompts per request.** Choose the most relevant ones.

For each recommended prompt, provide in user's input language:

```markdown
### [Prompt Title]

**Description**: [Brief description translated to user's language]

**Prompt (English - use this for generation)**:
```
[Original English prompt from content field]
```

**Sample Images**:
![Sample 1](sourceMedia[0])
![Sample 2](sourceMedia[1])

**Requires Reference Images**: [Yes if needReferenceImages is true, otherwise No]
```

### Step 4: Handle No Match

If no suitable prompts found:
1. Load `featured.json` completely
2. Analyze patterns from featured examples
3. Create a custom prompt based on:
   - Similar prompts in featured collection
   - User's specific requirements
   - Best practices from examples

## Prompt Data Structure

```json
{
  "content": "English prompt text for image generation",
  "title": "Prompt title",
  "description": "What this prompt creates",
  "sourceMedia": ["image_url_1", "image_url_2"],
  "needReferenceImages": false
}
```

## Language Handling

- Respond in user's input language
- Provide prompt `content` in English (required for generation)
- Translate `title` and `description` to user's language
