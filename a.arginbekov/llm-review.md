# LLM review record

## Review metadata

- Model: GPT-6 Codex (the model used in this Codex task)
- Prompt: `Review spec.md for ambiguity and missing behavior. Check whether each rule has an input with an expected output that distinguishes it from another plausible interpretation. Report concrete issues only.`
- Review performed: 2026-10-19

## Last review output

> The rules define deterministic tokenization, normalization, counting, sorting, and truncation. Two edge-case decisions need explicit examples to satisfy the “input and expected output for each decision” requirement: demonstrate a combining mark that remains after NFC acting as a separator, and show the specified failure for a non-string argument. The rest of the rules have distinguishing examples, including a tie resolved independently of first occurrence and an 11-word cutoff.

## Changes made in response

Added examples for `q\u0301q`, where the combining mark remains after NFC and separates two occurrences of `q`, and for integer input `42`, which raises `TypeError`. The other examples cover English, Russian, Kazakh, punctuation, digits, apostrophes, normalization, ties, empty results, and the ten-item limit.

## Output check

I checked the examples against the specification by applying its stated sequence: NFC, case folding, NFC, splitting on non-letter code points, counting, sorting by descending count and ascending Python string order, then taking at most ten. This is a manual consistency check, not execution of an implementation.
