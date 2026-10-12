# humanize - Ecko Std Lib Package

Durations, byte sizes, relative times, ordinals and counted words, rendered
for people to read - in CLI output, digests and logs.

Pure computation - no capabilities. This is the `std.humanize` module from the
Ecko standard library, moved into a package: every function answers exactly
what the native one did, so switching is the install line and nothing else.

## Install

```bash
ecko get github.com/ecko-lang/humanize
```

```ecko
import humanize
```

## Usage

```ecko
humanize.duration(7505000)                  # "2h 5m"   (milliseconds)
humanize.duration(250)                      # "250ms"
humanize.size(1468006)                      # "1.4 MB"
humanize.relative(event, time.now())        # "3 days ago" / "in 5 minutes" / "just now"
humanize.ordinal(23)                        # "23rd"
humanize.plural(3, "file")                  # "3 files"
humanize.plural(2, "person", "people")      # "2 people"
```

## API

| function | result |
|---|---|
| `duration(ms)` | The two largest non-zero units, e.g. `"2h 5m"`; under a second, `"250ms"`; zero is `"0s"`. A negative duration renders as its magnitude. |
| `size(bytes)` | 1024-based, one decimal with a trailing `.0` dropped: `"512 B"`, `"1 KB"`, `"1.4 MB"`, up to PB. A tie rounds to even (`535808` is `"523.2 KB"`). |
| `relative(event, now)` | Two unix-millisecond timestamps. A later `now` is the past (`"3 days ago"`), an earlier one the future (`"in 2 hours"`); within five seconds is `"just now"`. Months are 30 days, years 365. |
| `ordinal(n)` | `"1st"`, `"2nd"`, `"11th"`, `"23rd"`. |
| `plural(n, singular, plural?)` | `"3 days"`: the word gets an `s` for anything but 1 or -1, unless you pass the plural. |

A Float argument is truncated toward zero first, as the native module did. A
value that is not a number is refused with `{ kind: "bug", message }` naming
the function.

## Notes

- **Moving from `std.humanize`:** replace `import std.humanize` with
  `import humanize` after `ecko get`. The answers are the same - the package
  was checked against the native module on 6,052 inputs to every function,
  including the extremes of the Int range, with no difference.
- Durations are **milliseconds**, as `std.humanize.duration` took from Ecko
  0.58 until the module was removed in 0.61. Before 0.58 the native function
  took seconds; this package does not.
- `plural` only adds `s`. For English pluralisation rules and irregulars, use
  the [inflect](https://github.com/ecko-lang/inflect) package's `pluralize`.

## Testing

```bash
ecko test
```

Offline and deterministic. Every expected value in `tests/humanize_test.ecko`
was produced by the native `std.humanize` and frozen.

## License

MIT
