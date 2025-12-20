# Publishing to the Raycast Store

Prepare, validate, and publish your extension.

## Pre-Submission Checklist

- [ ] Run `npm run build` without errors
- [ ] Run `npm run lint` without errors
- [ ] Using latest `@raycast/api` version
- [ ] License set to `MIT`
- [ ] Author is your Raycast username
- [ ] Icon is 512x512 PNG
- [ ] All commands have descriptions
- [ ] Tested in both light and dark modes

## Naming Conventions

### Extension Title

Follow Apple Style Guide (Title Case):

✅ Good:

- `GitHub Repository Search`
- `Notion Page Creator`
- `Spotify Controls`

❌ Bad:

- `github search` (lowercase)
- `My awesome extension` (not descriptive)
- `Utils` (too generic)

### Command Titles

Use Verb + Noun structure:

✅ Good:

- `Search Repositories`
- `Create Issue`
- `Open Settings`

❌ Bad:

- `Repositories Search` (wrong order)
- `New Issue` (missing verb)
- `GitHub` (just service name)

### Subtitles

Add context, usually service name:

```json
{
  "name": "search",
  "title": "Search Repositories",
  "subtitle": "GitHub"
}
```

Don't use subtitles as descriptions.

## Icon Requirements

- **Size**: 512x512 pixels
- **Format**: PNG
- **Location**: `assets/icon.png`

### Light/Dark Variants

```
assets/
├── icon.png        # Light mode
└── icon@dark.png   # Dark mode
```

Reference in package.json:

```json
{
  "icon": "icon.png"
}
```

Raycast automatically uses `@dark` variant in dark mode.

### Icon Generator

Use [icon.ray.so](https://icon.ray.so) to create icons.

## README

Required if your extension needs setup (API keys, configuration).

```markdown
# My Extension

Brief description of what the extension does.

## Setup

1. Get your API key from [service.com/settings](https://service.com/settings)
2. Open Raycast Preferences → Extensions → My Extension
3. Enter your API key

## Features

- Feature 1
- Feature 2

## Commands

### Search Items
Search through all your items.

### Create Item
Create a new item with title and description.
```

Place images in `media/` folder (not `assets/`).

## Screenshots

### Requirements

- **Size**: 2000x1250 pixels
- **Ratio**: 16:10
- **Format**: PNG
- **Location**: `metadata/` folder
- **Maximum**: 6 screenshots

### Taking Screenshots

1. Set up Window Capture hotkey in Raycast Preferences → Advanced
2. Run your extension in dev mode
3. Open the command you want to capture
4. Press the hotkey, enable "Save to Metadata"

### Do's and Don'ts

✅ Do:

- Use consistent background across all screenshots
- Show the most useful commands
- Use Raycast Wallpapers for nice backgrounds

❌ Don't:

- Use different backgrounds per screenshot
- Include sensitive data
- Show other applications
- Mix light and dark themes

## CHANGELOG.md

Document changes for each release:

```markdown
# My Extension Changelog

## [New Feature] - {PR_MERGE_DATE}

- Added search functionality
- Improved loading performance
- Fixed bug with empty states

## [Initial Release] - {PR_MERGE_DATE}

- First release with basic features
- List and create items
- Sync with API
```

Use `{PR_MERGE_DATE}` placeholder - it's replaced when PR merges.

## Validation

### Build Check

```bash
npm run build
```

Must complete without errors. Creates optimized production build.

### Lint Check

```bash
npm run lint
```

Checks code style and best practices.

## Publishing

### First Time

```bash
npm run publish
```

This will:

1. Validate your extension
2. Build for production
3. Open a PR in [raycast/extensions](https://github.com/raycast/extensions)
4. Authenticate with GitHub if needed

### Updates

Same command:

```bash
npm run publish
```

Pushes to existing PR or creates new one.

### Manual Alternative

1. Fork [raycast/extensions](https://github.com/raycast/extensions)
2. Add your extension to `extensions/` folder
3. Create PR to `main` branch

## Review Process

After submitting:

1. Automated checks run (build, lint)
2. Raycast team reviews
3. May request changes
4. Once approved, PR is merged
5. Extension appears in Store within hours

### Common Review Feedback

- Missing or incorrect icon
- Generic extension name
- Missing command descriptions
- Hardcoded values that should be preferences
- Missing error handling
- Accessibility issues

## Contributing to Existing Extensions

### When to Contribute

- Small improvements (new action, preference)
- Bug fixes
- Non-breaking changes

### When to Create New

- Significantly different functionality
- Would break existing workflows
- Different configuration requirements

### Fork Extension

1. Find extension in Raycast
2. Use "Fork Extension" action
3. Make changes
4. Run `npm run publish`

You'll be added to `contributors` in package.json.

## Categories Reference

| Category | Examples |
|----------|----------|
| Applications | App launchers, app controls |
| Communication | Slack, Discord, Email |
| Data | Generators, converters |
| Design Tools | Figma, Sketch |
| Developer Tools | GitHub, npm, Docker |
| Documentation | API docs, wikis |
| Finance | Crypto, stocks, banking |
| Fun | Games, jokes |
| Media | Spotify, YouTube |
| News | Hacker News, RSS |
| Productivity | Todoist, Notion |
| Security | 1Password, encryption |
| System | Clipboard, files |
| Web | Bookmarks, URL tools |

## Post-Publishing

### Share Your Extension

```bash
# Copy store link
⌘ + ⌥ + . in Manage Extensions
```

Share on:

- [Raycast Community Slack](https://raycast.com/community)
- Twitter/X
- Your team

### Monitor Issues

Check [raycast.com/extension-issues](https://raycast.com/extension-issues) for crash reports and errors from users.

### Update Regularly

- Keep dependencies updated
- Respond to user feedback
- Fix reported issues promptly
