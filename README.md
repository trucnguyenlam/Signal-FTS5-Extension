# Overview

[Signal-FTS5-Extension](https://darksi.de/13.sqlite-fts5-structure/) is a C ABI library which exposes a
[FTS5](https://www.sqlite.org/fts5.html) tokenizer function named
`signal_tokenizer` that:

- Segments UTF-8 strings into words according to
  [Unicode standard](http://www.unicode.org/reports/tr29/)
- Normalizes and removes diacritics from words
- Converts words to lower case

When used as a custom FTS5 tokenizer this enables application to support CJK
symbols in full-text search.

# Extension Build/Usage Example

```sh
cargo rustc --features extension -r --crate-type=cdylib
```

Load extension from `./target/release/libsignal_tokenizer.dylib`, note that you might need to specify entry point `sqlite3_signaltokenizer_init`

```sql
CREATE VIRTUAL TABLE
fts
USING fts5(content, tokenize='signal_tokenizer')
```

To build dynamic library for use in Android, please set up [cargo-ndk](https://github.com/bbqsrc/cargo-ndk), then you can build with this command

```sh
cargo ndk -t armeabi-v7a -t arm64-v8a -t x86 -t x86_64 -o ./jniLibs build --release
```

To build dynamic library for use in Android, please set up [xcframework](https://github.com/trucnguyenlam/xcframework), then you can build with this command

```sh
xcframework --release --features extension
```

# Generating headers

```sh
cbindgen --profile release . -o target/release/fts5-tokenizer.h
```

# Legal things

## License

Copyright 2023 Signal Messenger, LLC.

Licensed under the AGPLv3: http://www.gnu.org/licenses/agpl-3.0.html

