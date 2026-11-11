# Changelog

## 0.2.0

### Fixed

- `ordinal` of a negative number takes the suffix of its magnitude: `ordinal(-1)` is `"-1st"` (was
  `"-1th"`), `ordinal(-22)` is `"-22nd"`. This matches the `inflect` package.
- `size` moves to the next unit when rounding reaches 1024: `size(1048575)` is `"1 MB"` (was
  `"1024 KB"`).
- `size` keeps the sign of a negative count: `size(-2048)` is `"-2 KB"` (was `"2 KB"`).
- `plural` uses the regular English endings: `plural(2, "bus")` is `"2 buses"` (was `"2 buss"`),
  `plural(2, "city")` is `"2 cities"`.
- An infinite Float no longer fails inside a helper; it is refused like any other bad argument.

### Breaking

- `ordinal` refuses a fractional number instead of truncating it: `ordinal(21.9)` was `"21st"`.
  Migration: truncate first, `ordinal(int(x))`, if that is what you meant.
- Errors carry `reason: "bad_argument"`, and the message reads `humanize.<fn>: <arg> must be ...`
  instead of `humanize.<fn> needs a number of ...`.
  Migration: match on `get(e, "reason")` rather than the message text.
