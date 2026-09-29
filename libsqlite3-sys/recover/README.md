# SQLite recovery

These unmodified sources come from SQLite 3.45.3, commit
`b74eb00e2cb05d9749859e6fbe77d229ad1dc1e1`, directory `ext/recover`:
https://github.com/sqlite/sqlite/tree/b74eb00e2cb05d9749859e6fbe77d229ad1dc1e1/ext/recover

The optional `recovery` feature compiles them alongside bundled SQLCipher and
exposes four recovery functions. When updating SQLCipher, update these sources
to match the SQLite version in `sqlcipher/sqlite3.h`.

Bindings are checked in, like the existing SQLite bindings. From `libsqlite3-sys`,
regenerate them with bindgen-cli 0.69.5 and libclang:

```sh
bindgen recover/sqlite3recover.h \
  --allowlist-function 'sqlite3_recover_(init_sql|run|errmsg|finish)' \
  --blocklist-type sqlite3 --raw-line 'use crate::sqlite3;' \
  --no-layout-tests --no-doc-comments \
  --output src/recovery.rs -- -Isqlcipher
```

AGI consumes this fork by Git revision; the crate retains its upstream version.
