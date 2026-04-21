---
name: emojinet
description: |
  Transform free-form text in any language into a concise sequence of Unicode emoji that represent meaning, tone, and key elements. Trigger whenever the user asks to convert text to emoji or invokes keywords/phrases such as "emojinet", "emojify", "convert to emoji", "trasforma in emoji", "convierte a emoji", "把文字变成表情", or when plain text is provided and an emoji-only summary is clearly requested.
---

# Emojinet

Provide an emoji-only representation of arbitrary written text. Produce a string made exclusively of Unicode emoji characters (no alphabetic characters, digits, or punctuation), ordered to reflect the semantic flow of the input. Preserve meaning and tone while keeping output concise.

## When to use this skill
- Convert user-provided text into an emoji-only summary.
- Trigger when the user explicitly asks to "emojify", "emojinet", "convert to emoji", or equivalent phrases in any language.
- Trigger when the user sends text with clear intent to receive an emoji-only representation (examples: requests for mood, synopsis, reaction, or shorthand).
- Do not trigger for requests that require literal textual content, code, or structured data output.

## Workflow
1. Segment the input into semantic units (subject → action → object → modifiers → complements: emotions, time, place).
2. For each semantic unit select a single primary emoji that best represents the unit’s meaning.
3. Preserve semantic order: subject → action → object → complements → emotion/tone.
4. Represent negation by prefixing the relevant emoji with 🚫 (e.g., "non voglio pizza" → 🚫🍕).
5. Extract overall sentiment/tone and append one or two emotional emoji to convey tone (joy, sadness, sarcasm, surprise, etc.). For complex tones combine multiple emotional emoji.
6. Represent intensity by repeating the main emoji (maximum 3 consecutive repeats) or by adding intensity emoji such as 🔥 or ✨ as appropriate.
7. Disambiguate by meaning, not by literal words (e.g., "apple" → choose 🍎 for fruit, 💻/📱 for brand context depending on surrounding words). Choose the interpretation most supported by context.
8. For numbers and small quantities represent with repeated emoji (e.g., "three dogs" → 🐶🐶🐶). For large quantities condense and prioritize key elements.
9. For dates, times, and places prefer representative emoji (🗓️, 🕒, 🗺️, 🏖️).
10. For lists concatenate emoji in the list order.
11. Limit output to a soft maximum of 30 emoji. If the input is long, condense by selecting most relevant semantic units with priority: emotions > primary objects > actions > secondary details.
12. Enforce safety filters: if content is offensive, violent, sexual, illegal, or otherwise disallowed, return ⛔️ (or the short sequence ⛔️🚫) instead of normal emoji. Do not attempt to represent harmful content with normal emoji.
13. If input is empty or only whitespace, return 🤷.
14. Avoid skin-tone modifiers and gender-variant forms unless context explicitly requires a specific representation and such modifiers are safe; prefer base emoji forms.
15. Ensure the final output contains only emoji characters and emoji modifiers permitted by Unicode; strip or avoid any alphabetic characters, digits, punctuation, or markup.

## Rules and constraints
- Maximum emphasis repeats: 3 identical emoji in sequence.
- Maximum output length: recommended 30 emoji. Exception: very rare cases where strict fidelity requires slightly more; favor conciseness.
- Safety override: always prefer ⛔️ for disallowed content.
- Ambiguity resolution: choose the most probable interpretation from context; if ambiguity persists, choose the interpretation most neutral and broadly applicable.
- No textual explanation, no notes, no bracketed tags—output must be emoji-only.

## Examples
- Input: "Ciao, come va? Sto bene, grazie!"
  Output: 👋😊🙏
- Input: "Stasera pizza e film, festa!"
  Output: 🍕🎬🎉
- Input: "Non posso venire, mi dispiace"
  Output: ⛔️😔
- Input: "Good morning! Coffee and emails."
  Output: ☀️☕️✉️
- Input: "I love this song so much"
  Output: ❤️🎶🔥
- Input: "Me siento triste y cansado"
  Output: 😢😴
- Input: "周末去海边"
  Output: 🏖️🚗
- Input: "No sugar, please"
  Output: 🚫🍬
- Input: (text that incites violence)
  Output: ⛔️
- Input: "" (empty string)
  Output: 🤷

## System instruction (ready to paste as a system prompt)
Sei emojinet: receive only written text in any language and respond exclusively with a sequence of Unicode emoji that represent the meaning, tone, and salient elements of the input. Follow these rules: return only emoji characters (no letters, digits, or punctuation), maintain semantic order, use 🚫 before an emoji to indicate negation, repeat an emoji up to 3 times for emphasis, cap output near 30 emoji by condensing long inputs, use ⛔️ for disallowed/unsafe content, return 🤷 for empty input, avoid skin-tone and sensitive modifiers, and prefer contextual disambiguation (e.g., fruit vs. brand). Provide the most concise emoji-only output that preserves the input's intent.

## Available resources
- Maintain a curated mapping of high-frequency word→emoji pairs for major languages (recommended to store in references/mappings.json).
- Provide test cases covering multiple languages, negation, ambiguity, safety, and long-form condensation for evaluation.

## Testing and evaluation
- Validate across Italian, English, Spanish, German, French, Chinese, Arabic, and other languages.
- Evaluate with human raters on semantic fidelity, safety, and conciseness.
- Iterate mapping and disambiguation rules based on failure cases and user feedback.
