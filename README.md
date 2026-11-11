# humanize

Durations, byte sizes, relative times, ordinals and counted words, rendered for
people to read in CLI output, digests and logs. Pure computation - no
capabilities.

## Install

```bash
ecko get github.com/ecko-lang/humanize
```

```ecko
import humanize
```

## Usage

```ecko
import humanize

print(humanize.duration(7505000))  # 2h 5m
print(humanize.duration(250))  # 250ms
print(humanize.size(1468006))  # 1.4 MB
print(humanize.ordinal(23))  # 23rd
print(humanize.plural(3, "box"))  # 3 boxes
print(humanize.plural(2, "person", "people"))  # 2 people

# `relative` takes "now" as an argument; pass time.now() in a real program.
now = 1700000000000
print(humanize.relative(now - 3 * 86400000, now))  # 3 days ago
print(humanize.relative(now + 2 * 3600000, now))  # in 2 hours
```

## API

| function | returns |
|---|---|
| `duration(ms)` | the two largest non-zero units, `"2h 5m"`; under a second `"250ms"`; zero `"0s"` |
| `size(bytes)` | a 1024-based size with one decimal, `"512 B"`, `"1 KB"`, `"1.4 MB"`, up to PB |
| `relative(event, now)` | `"3 days ago"`, `"in 2 hours"` or `"just now"`, between two unix-millisecond timestamps |
| `ordinal(n)` | the ordinal of a whole number, `"23rd"`, `"-11th"` |
| `plural(n, singular, plural = null)` | the count and the word, plural for anything but 1 and -1, `"3 days"` |

## Notes

### Errors

| kind | reason | when |
|---|---|---|
| `bug` | `bad_argument` | a number argument is not a number (or is an infinite Float), `ordinal` got a fractional number, or a word is not a string |

### Numbers

Durations are milliseconds. A negative duration renders as its magnitude; a
negative size keeps its sign (`"-2 KB"`). A Float is truncated toward zero
first, except in `ordinal`, which accepts only a whole number and gives the
same answer as the `inflect` package (`-1st`).

Sizes use 1024-based units labelled KB, MB, GB, TB and PB. A tie rounds to even
(`535808` is `"523.2 KB"`), and a value that rounds up to 1024 moves to the
next unit (`1048575` is `"1 MB"`).

`relative` counts months as 30 days and years as 365.

### Plurals

`plural` follows the regular English endings: `-es` after s, x, z, ch and sh,
`-ies` after a consonant and y, `-s` otherwise. Pass the plural for anything
irregular, or use the [inflect](https://github.com/ecko-lang/inflect) package,
which knows irregular words.

### Moving from `std.humanize`

Replace `import std.humanize` with `import humanize` after `ecko get`. The
answers are the native module's, except where it was wrong: negative ordinals
(`-1th` is now `-1st`), a size rounding into the next unit (`1024 KB` is now
`1 MB`), the sign of a negative size, the regular plural endings, and
`ordinal` of a fractional number, which is now refused.

## Testing

```bash
ecko test
```

## License

[MIT](LICENSE)
