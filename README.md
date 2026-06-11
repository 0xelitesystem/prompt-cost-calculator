# prompt-cost-calculator

Estimate token count and API cost for a prompt across Anthropic, OpenAI, and Google models. Browser-only. No API calls. No tracking. Single HTML file.

**Live demo:** https://0xelitesystem.github.io/prompt-cost-calculator/

## Why

Most LLM users run prompts blind and get surprised by the bill. Most token counters online are provider-specific and skip the cost. This shows both, side-by-side, across providers.

Useful for:

- **Pricing decisions** before committing to a provider
- **Prompt optimization**: did your refactor make it cheaper?
- **Budget estimation**: "If I run this 1,000 times, what does it cost?"
- **Context-window checks**: a quick warning when you're about to overflow

## Use it

Open `index.html` in any browser. Or visit the hosted version at `https://0xelitesystem.github.io/prompt-cost-calculator/` once GitHub Pages is enabled.

1. Paste your prompt in the Input box.
2. Optionally paste an expected output in the second box (for round-trip cost).
3. Pick a provider tab. The cost grid recalculates instantly.

No API key required. Nothing leaves your browser.

## How tokens are counted

This tool uses **character-based approximation**, not the actual tokenizers. Each provider uses a different tokenizer:

- Anthropic: BPE variant (close to ~4 chars/token for English)
- OpenAI: tiktoken (close to ~4 chars/token for English)
- Google: SentencePiece (slightly more efficient on English, ~4.5 chars/token)

The approximation factors are calibrated for typical English prose. Expect estimates within roughly **5 to 15 percent** of the real count for natural language. For code, JSON, or other structured input the variance is higher.

For exact counts, use each provider's official tokenizer (Anthropic's `count_tokens` API, OpenAI's `tiktoken` library, Google's tokenizer endpoint).

This trade is intentional: bundling the real tokenizers would mean a heavy build process and ~5MB of data per provider. The approximation is good enough for budget decisions.

## Pricing accuracy

Pricing values reflect publicly listed rates per 1M tokens at time of build. Verify in each provider's billing console before relying on cost numbers for real budget decisions.

To update pricing, edit the `PROVIDERS` object at the top of the script block in `index.html`. It's the only place pricing values appear.

## Models supported

**Anthropic:** Opus 4.7, Opus 4.6, Sonnet 4.6, Haiku 4.5
**OpenAI:** GPT-4o, GPT-4o mini, GPT-4 Turbo, GPT-3.5 Turbo
**Google:** Gemini 1.5 Pro, Gemini 1.5 Flash, Gemini 1.0 Pro

To add or remove models, edit the relevant `models` array. Each entry is `{id, name, tier, inputPer1M, outputPer1M, contextWindow}`.

## Hosting your own

This is a static HTML file. Deploy options:

- **GitHub Pages**: push to a repo, enable Pages, done.
- **Netlify / Vercel / Cloudflare Pages**: drag and drop or `git push`.
- **Any static host**: works on S3, Firebase Hosting, surge.sh, anything.

## Tech

- Single HTML file, ~600 lines including CSS and JS
- Vanilla JS, no frameworks, no dependencies, no build step
- Tested in current Chrome, Firefox, Safari
- Light and dark themes, OS preference honored on first load
- Full keyboard navigation (arrow keys cycle tabs)
- WCAG AA color contrast on both themes

## What it doesn't do

- Doesn't call any LLM API. There's nothing to call.
- Doesn't store your prompt anywhere. Refreshing the page wipes it.
- Doesn't analyze prompt content for safety, sensitivity, or quality. That's not the job.
- Doesn't compare across providers in a single view; pick a tab. Future versions may add a unified compare view.

## License

MIT. See [LICENSE](LICENSE).

## Related

- [claude-eval-harness](https://github.com/0xelitesystem/claude-eval-harness), actually run the prompt against multiple models
- [byok-patterns](https://github.com/0xelitesystem/byok-patterns), BYOK reference implementations
- [prompt-templates](https://github.com/0xelitesystem/prompt-templates), production prompts targeting LLM failure modes
- [readme-slop-checker](https://github.com/0xelitesystem/readme-slop-checker), audit a README for AI cliches
