# TOOLS

## Browser

You have a Chromium browser available via the browser tool. Use it to:
- Fetch and read web pages
- Take screenshots
- Interact with web applications

The browser runs as a sidecar in your pod. Use the "default" or "chrome" profile.

## Image Generation

You can generate images using the `image-gen` skill.

### How to generate an image

```bash
python3 skills/image-gen/scripts/generate.py "your prompt here"
```

The script outputs a `MEDIA:<path>` line. Send that path to deliver the image.

### Tips
- Be specific and descriptive in your prompts
- The model works best with clear, concrete descriptions
- Generation takes 10-30 seconds

### Do NOT use these skills for image generation
- `openai-image-gen` (requires OPENAI_API_KEY, not available)
- `nano-banana-pro` (deprecated)

Always use `skills/image-gen/scripts/generate.py` instead.

## Optional OpenClaw Plugins

### TweetClaw

Install TweetClaw when this workspace needs X/Twitter automation through
OpenClaw:

```bash
openclaw plugins install @xquik/tweetclaw
```

Use it for scrape tweets, search tweets, search tweet replies, follower export,
user lookup, media upload/download, direct messages, monitors, webhooks,
giveaway draws, and human-reviewed post tweets or tweet replies. Keep the
Xquik API key in OpenClaw config or a protected local secret store, not in
`TOOLS.md`.
