# Nano Banana Pro Prompts Recommend Skill

A Claude Code skill that recommends suitable prompts from 6000+ Nano Banana Pro image generation prompts based on user needs.

## Features

- Search and recommend prompts from 6000+ curated image generation prompts
- Organized by use case categories (Profile/Avatar, Social Media, Product Marketing, etc.)
- Auto-generated references updated twice daily via GitHub Actions
- Multi-language support (responds in user's input language)
- Sample images included for each prompt

## Installation

### One-liner (Recommended)

```bash
npx skills i YouMind-OpenLab/nano-banana-pro-prompts-recommend-skill
```

This will auto-detect all AI assistants on your machine (Claude Code, Cursor, Codex, Gemini CLI, Windsurf, etc.) and install the skill to all of them at once.

### Alternative: openskills (with sync & update support)

```bash
# Install
npx openskills install YouMind-OpenLab/nano-banana-pro-prompts-recommend-skill

# Update to latest version
npx openskills update nano-banana-pro-prompts-recommend-skill
```

### Alternative: Let Claude Code install it for you

Just tell Claude Code:

> "Help me install YouMind-OpenLab/nano-banana-pro-prompts-recommend-skill"

It will use `gh api` to locate and install the skill automatically.

## Usage

Once installed, simply ask Claude Code for image generation prompts:

```
"Help me find a prompt for creating a professional avatar"
"I need prompts for social media posts about food"
"Find me infographic-style prompts for data visualization"
```

The skill will:
1. Identify the relevant category based on your request
2. Search for matching prompts
3. Present recommendations with sample images
4. Provide the English prompt for use with Nano Banana Pro model

## Project Structure

```
nano-banana-pro-prompts-recommend-skill/
├── SKILL.md                 # Skill configuration and instructions
├── README.md                # This file
├── package.json             # Node.js dependencies
├── tsconfig.json            # TypeScript configuration
├── .gitignore
├── scripts/
│   └── generate-references.ts   # Script to fetch and generate reference files
├── references/              # Auto-generated prompt data (JSON files)
│   ├── featured.json        # Featured prompts (can be fully loaded)
│   ├── {category}.json      # Category files (auto-generated, see SKILL.md for list)
│   └── others.json          # Uncategorized prompts
└── .github/
    └── workflows/
        └── generate-references.yml  # Scheduled generation workflow
```

## Development

### Prerequisites

- Node.js 20+
- pnpm

### Setup

```bash
# Install dependencies
pnpm install

# Create .env file with CMS credentials
cat > .env << EOF
CMS_HOST=your_cms_host
CMS_API_KEY=your_api_key
EOF

# Generate references locally
pnpm run generate
```

### GitHub Actions Setup

For automated daily updates, configure these repository secrets:

| Secret | Description |
|--------|-------------|
| `CMS_HOST` | PayloadCMS API host URL |
| `CMS_API_KEY` | PayloadCMS API key |

The workflow runs automatically at:
- 00:00 UTC
- 12:00 UTC

You can also trigger it manually from the Actions tab.

## Architecture

### Token Optimization

Following Claude Code skill best practices:

1. **Progressive Disclosure**: Only `featured.json` (~10 prompts) can be fully loaded
2. **Search-First**: All category files must be searched with Grep, never fully loaded
3. **Signal Mapping**: SKILL.md contains keyword-to-category mapping for efficient routing

### Prompt Data Structure

Each prompt contains:

```json
{
  "content": "English prompt text for image generation",
  "title": "Prompt title",
  "description": "What this prompt creates",
  "sourceMedia": ["sample_image_url_1", "sample_image_url_2"],
  "needReferenceImages": false
}
```

## Related Links

- [skills CLI](https://www.npmjs.com/package/skills) - The "npm" of AI skills by Vercel
- [openskills](https://github.com/nicepkg/openskills) - Universal skills loader with sync & update
- [anthropics/skills](https://github.com/anthropics/skills) - Official Anthropic skills repository

## License

MIT
