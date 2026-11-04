# TOV compiler and settings block

How settings become writing directives. All data lives in `tov-schema.json`; this file describes the procedure. The reference implementation is `src/lib/tov/compiler.ts` (sliders and claim mode) and `src/lib/settings.ts` (everything else, and the block). Use this file when the user changes a setting in chat, when no block was pasted, or when a block's schema number differs from the schema file's `version`.

## 1. Sliders

- A missing slider takes its `default`. Values are clamped to 0–100 and rounded.
- Levels: 0–33 `low`, 34–66 `medium`, 67–100 `high` (inclusive boundaries).
- Each slider contributes `directives[level]`, in schema order.

## 2. Conflict rules (`conflict_rules`)

A rule fires when every slider in `when` is at the given level and, if `modes` is set, the claim mode is one of them. It appends its `directive`, adds its `avoid` items, and may raise a `warning`. Its resolution beats the sliders' own directives.

| Rule | When | Resolution |
|---|---|---|
| `formal-humor` | formality high, humor high | Restrained wit, no puns or slapstick. |
| `hype-no-specificity` | hype high, specificity low, mode free_hype or hype_with_anchors | Stay bold; do not weaken superlatives because proof is missing. |
| `hype-no-specificity-strict` | same, mode strict_claims | Energy through rhythm and benefits; list the proof needed under Gaps. Warns. |
| `premium-punchy` | premium high, punchiness high | Concise and elegant; no exclamation marks or stacked fragments. |
| `human-hype` | humanization high, hype high | Energetic but credible: one excited person with concrete detail. |
| `urgency-no-proof` | urgency high | Deadlines and scarcity only from the brief. |
| `casual-authority` | authority high, formality low | Seasoned colleague, plain words. |
| `calm-energy-urgent` | energy low, urgency high | Urgency through stakes, not volume. |
| `understated-bold` | hype low, confidence high | Quiet certainty, no superlatives. |
| `emotion-simple` | emotion high, simplicity high | Emotion through plain words and one image. |

## 3. Directive order

1. Slider directives (14), then fired conflict resolutions.
2. Claim mode directives (`claim_intensities[mode].directives`), the `never_invent` rule, `common_directives`.
3. Section intensity: headline or body offset of ±1 shifts Hype by ±15 points for that part (clamped 0–100); then the CTA style directive.
4. Claim types: for each of the four types, the `on` text if allowed, else the `off` text.
5. Audience directive.
6. Channel structure and CTA (none for channel `any`).
7. Sector line, only when the claim check is on.
8. Language note, market adaptation, and French `tu`/`vous` when set.
9. Approved words (minus any that are also banned), banned words, the voice-examples line when examples exist.
10. Length (`max_words` or the channel default, clamped 20–1000), then the output mode (`primary`: primary copy only; `single`: primary plus 1 to 3 alternatives, each with an optional own length given as `alt_max_words`, default 1 alternative; or the `comparison` set), then the claim check directive.

## 4. Avoid list

Claim mode `avoid`, then `always_avoid`, then conflict-rule `avoid`, then `conditional_avoid` items whose slider is at the given level. Duplicates are removed. A low Hype slider adds "Superlatives" even in `free_hype`: an understated slider is an explicit request.

## 5. Settings block format

```
<copywriter-settings schema="2">
preset: Default (bold premium)                      (only when the settings match a profile exactly)
language: fr (FR) · address: auto
channel: any · audience: general · sector: auto
claim_mode: free_hype · claim_types_allowed: emotional, comparative, performance, guarantee · claim_check: risky
output: primary · alternatives: 0 · max_words: 250
sections: headline=louder · body=same · cta=action
sliders: confidence=90 hype=90 premium=80 energy=60 formality=40 warmth=50 humor=15 simplicity=65 urgency=40 authority=60 emotion=50 specificity=75 punchiness=80 humanization=70
approved_words: exceptional, best ever, unmatched, obsessed, elite
banned_words: seamless, leverage, unlock, elevate, robust, delve

directives:
- ...

avoid:
- ...

voice_examples:                                     (only when supplied, indented two spaces)
  ...
</copywriter-settings>
```

The page strips any `copywriter-settings` tag from user text, so the block always opens and closes exactly once.

## 6. Worked contrast

| Slider | Premium bold | Natural & human |
|---|---|---|
| confidence / hype | 95 / 95 | 50 / 12 |
| premium / punchiness | 85 / 88 | 30 / 40 |
| humanization / warmth | 60 / 35 | 95 / 80 |
| formality / simplicity | 55 / 55 | 12 / 88 |

Premium bold compiles to decisive, unmuted superlatives in short, elegant lines with no exclamation marks. Natural & human compiles to understatement, contractions, the audience's vocabulary and a warm acknowledgement of the reader's situation. The two directive lists differ in at least 10 of 14 slider lines.
