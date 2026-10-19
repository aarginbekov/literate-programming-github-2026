# LLM review record

## Review metadata

- Model: GPT-6 Codex (the model used in this Codex task)
- Review prompt: `Review spec.md for ambiguity and missing behavior. Check whether each rule has an input with an expected output that distinguishes it from another plausible interpretation. Report concrete issues only.`
- Review performed: 2026-10-19

## Last review output

> The rules define deterministic tokenization, normalization, counting, sorting, and truncation. Two edge-case decisions need explicit examples to satisfy the “input and expected output for each decision” requirement: demonstrate a combining mark that remains after NFC acting as a separator, and show the specified failure for a non-string argument. The rest of the rules have distinguishing examples, including a tie resolved independently of first occurrence and an 11-word cutoff.

## Changes made in response

Added examples for `q\u0301q`, where the combining mark remains after NFC and separates two occurrences of `q`, and for integer input `42`, which raises `TypeError`. The other examples cover English, Russian, Kazakh, punctuation, digits, apostrophes, normalization, ties, empty results, and the ten-item limit.

## Generated-code check

I gave the specification to GPT-6 Codex and asked it to implement the stated rules. I ran its Python 3 implementation against every example in `spec.md`; all 10 output examples matched, and the integer input raised `TypeError` as specified. The implementation used for the check was:

```python
import unicodedata

def top_words(text):
    if not isinstance(text, str):
        raise TypeError("text must be str")
    normalized = unicodedata.normalize(
        "NFC", unicodedata.normalize("NFC", text).casefold()
    )
    counts = {}
    current = []
    for char in normalized:
        if unicodedata.category(char).startswith("L"):
            current.append(char)
        elif current:
            word = "".join(current)
            counts[word] = counts.get(word, 0) + 1
            current = []
    if current:
        word = "".join(current)
        counts[word] = counts.get(word, 0) + 1
    return sorted(counts.items(), key=lambda item: (-item[1], item[0]))[:10]
```
