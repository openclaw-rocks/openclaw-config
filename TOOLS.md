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
