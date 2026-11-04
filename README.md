# TOV Copywriter

**A Claude skill that bypasses the flat, soulless style of LLM copy.** It writes, rewrites and reviews marketing copy under tone-of-voice control: bold by default, human-sounding, and never inventing a statistic, review, award, certification or guarantee.

**Settings page:** [ailive.fr/copywriter](https://www.ailive.fr/copywriter/)  
**Skill:** [`copywriter-tov/`](./copywriter-tov) in this repo

The page and the skill work together. The page builds a settings block. The skill reads it and writes accordingly.

## What it solves

| Problem with default LLM copy | What TOV Copywriter does |
|---|---|
| Interchangeable, "could be any brand" phrasing | Fact-locked specificity: named features, exact numbers, concrete scenes, all taken from your brief |
| Stock AI words and rhythm (*seamless, leverage, unlock, elevate*) | Banned-word lists, structural AI-tell rules, uneven human rhythm |
| One volume only, either timid or hype | 14 sliders (confidence, hype, premium, energy, formality, warmth, humor, simplicity, urgency, authority, emotion, specificity, punchiness, humanization) plus separate intensity for headlines and body |
| Invented proof to make copy "convert" | Never invents verifiable facts; missing proof is listed under *Gaps* instead |
| No idea what is legally risky | Claim check after the copy: risk per claim, safer wording, what would make it safe (EU / France focus). A recommendation only, the copy is never blocked |
| Lost voice across channels and languages | Channel structure (landing page, ads, email, social, scripts) and transcreation between French, English and Hebrew |

## How to use it

1. **Install the skill.** Add the `copywriter-tov` folder to Claude.
   - Claude Code: copy it to `~/.claude/skills/copywriter-tov/` (or `.claude/skills/` in a project).
   - Claude.ai: zip the `copywriter-tov` folder and upload it under Settings → Capabilities → Skills.
2. **Open [ailive.fr/copywriter](https://www.ailive.fr/copywriter/).** Pick a profile, move the sliders, choose the claim mode, channel, language, audience, and your approved and banned words. Optionally paste 3 to 5 pieces of copy you love as voice examples.
3. **Click *Copy settings*.** Everything runs in your browser; nothing is sent anywhere.
4. **Paste the block into Claude, then your brief.** Fill in product, audience, main benefit, features, proof points, differentiators, objections and desired action. Missing fields are inferred or listed as gaps.
5. **Iterate in plain words.** "Redo only the headline", "more premium", "plus naturel", "tutoie", "no comparisons". Only the part you name changes.

You can also use the skill without the page: say "hype 40, more luxury, no comparisons" and it starts from the defaults and applies your change.

## What you can ask for

New copy from a brief · rewrites (fact-locked) · 8 to 15 headline, tagline or subject-line options grouped by angle · iteration on a single part · adaptation between French, English and Hebrew · review of existing copy for voice, AI tells and claim risk · Bold / Premium / Proof-led comparison sets.

## Files

| File | Role |
|---|---|
| [`copywriter-tov/SKILL.md`](./copywriter-tov/SKILL.md) | The skill: workflow, claim modes, persuasion strategies, self-review |
| [`copywriter-tov/tov-schema.json`](./copywriter-tov/tov-schema.json) | Single source of truth: sliders, presets, defaults, claim modes, channels, audiences |
| [`copywriter-tov/tov-compiler.md`](./copywriter-tov/tov-compiler.md) | How settings compile into directives, and the settings block format |
| [`copywriter-tov/claim-risk.md`](./copywriter-tov/claim-risk.md) | Risk notes by claim type and sector |

## Credits

Built by [Ailive](https://www.ailive.fr). Synthesised from conversion-copywriting principles, fact-lock humanizer rules, brief-reading and preflight discipline, and structure-over-word-swap editing.
