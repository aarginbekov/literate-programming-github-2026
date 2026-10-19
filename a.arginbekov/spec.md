# Specification: `top_words`

## Interface

Implement `top_words(text)` in `top_words.py` for Python 3. The argument must be a Python `str`. Return a list of `(word, count)` pairs, where each word is a string and each count is an integer. If `text` is not a string, raise `TypeError`.

## Word and counting rules

1. Normalize the input to Unicode NFC, apply Unicode `casefold()`, then normalize to NFC again.
2. A word is a maximal, nonempty sequence of Unicode code points whose Unicode general category begins with `L` (a letter). Every other code point is a separator. In particular, whitespace, punctuation, apostrophes, hyphens, digits, symbols, and emoji separate words; they are not included in a word. A combining mark that remains after NFC is also a separator.
3. Count each resulting word exactly as it appears after normalization and case folding. Do not remove accents, stem words, or otherwise alter spelling.
4. Order results by count descending. When counts are equal, order words by ascending Unicode code point sequence (the ordinary Python string ordering).
5. Return at most the first 10 words in that order. Return every word if fewer than 10 occur. Return an empty list if no words occur.

## Examples

The notation `\u0301` in an input denotes the single combining acute accent code point U+0301; it is included to make the normalization example unambiguous.

| Input | Expected output | Decision illustrated |
| --- | --- | --- |
| `Cat cat DOG!` | `[('cat', 2), ('dog', 1)]` | Case folding combines upper- and lowercase occurrences; punctuation separates words; fewer than 10 words are returned. |
| `go2go` | `[('go', 2)]` | A digit separates adjacent letter sequences. |
| `q\u0301q` | `[('q', 2)]` | A combining mark that remains after NFC is a separator. |
| `42` (integer) | raises `TypeError` | Non-string inputs are rejected rather than converted. |
| `can't can't` | `[('can', 2), ("t", 2)]` | Apostrophes are separators, so the contraction has two words. The tie is ordered alphabetically. |
| `Мир мирный МИР` | `[('мир', 2), ('мирный', 1)]` | Cyrillic letters are words and case folding is applied. |
| `ҚАЗАҚ қазақ` | `[('қазақ', 2)]` | Kazakh Cyrillic letters are words and case folding is applied. |
| `Cafe\u0301 café` | `[('café', 2)]` | NFC makes a decomposed and a precomposed accented spelling equivalent. |
| `z a` | `[('a', 1), ('z', 1)]` | Equal counts are ordered by Python string order, not by first occurrence. |
| `k j i h g f e d c b a` | `[('a', 1), ('b', 1), ('c', 1), ('d', 1), ('e', 1), ('f', 1), ('g', 1), ('h', 1), ('i', 1), ('j', 1)]` | Only the first 10 entries after sorting are returned. |
| `123 - ... 🙂` | `[]` | If there are no letter sequences, the result is empty. |


