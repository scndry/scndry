👋 Writing software since 2008, mostly Java backend since 2015. I keep ending up under the API surface — framework source, format specs, packet captures, kernel knobs — and that's the part I find most interesting.

### 🚀 Things I've shipped

- **A patch in MySQL Connector/J 8.0.32** ([Bug #108414](https://bugs.mysql.com/bug.php?id=108414)) — `BIT` was sitting in a commented-out block of the type switch, falling through to the string default and generating malformed `COM_STMT_EXECUTE` packets.
  Caught during a Spring Boot upgrade — Wireshark showed the malformed packets, the MySQL client/server protocol spec told me which field was wrong, and the Connector/J source confirmed it.
  Oracle was [kind enough to ship the fix](https://blogs.oracle.com/mysql/mysql-8032-thank-you-for-the-contributions) — moving `BIT` into the `BOOLEAN`/`TINYINT` group.
- **[jackson-dataformat-spreadsheet](https://github.com/scndry/jackson-dataformat-spreadsheet)** — A first-class Jackson dataformat module for Excel, built the way Jackson's own format modules are built.
  Extends `JsonFactory` / `ParserMinimalBase` / `GeneratorBase` directly, implements OOXML SpreadsheetML, and bypasses POI's User Model on the hot path.
  Listed in [FasterXML community modules](https://github.com/FasterXML/jackson#data-format-modules), on [Maven Central](https://central.sonatype.com/artifact/io.github.scndry/jackson-dataformat-spreadsheet).

### 🔥 War stories

- **`nf_conntrack` table filling up** — one HAProxy fronting every backend, all stats clean, only the kernel was logging it.
- **Tomcat threads stuck on keep-alive** — 508 of 512 AJP threads idle in `socketRead0()`, with the Acceptor blocked at `LimitLatch.countUpOrAwait()`. `jstack -F` showed it, Bugzilla #53173 explained it, NIO fixed it.