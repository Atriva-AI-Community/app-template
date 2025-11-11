# Licensing Decisions — Guidance

This document summarizes recommended license decisions and rationale.

## Short recommendations

- If **Atriva AI is sole copyright holder** for the code being released:
  - Prefer **Apache 2.0** if you want patent protection and stronger legal safeguards.
  - MIT is simpler and compatible with many users; it is acceptable if patent clause is not a priority.

- If the internal `atriva-ai` repo already contains code contributed by **others under MIT**:
  - Do **not** relicense those files to Apache 2.0 without contributors' consent.
  - Either: keep **MIT** for the public repo or only copy files you can relicense.

## What happens if you mix licenses?

- **Copy MIT-licensed files into an Apache 2.0 repo**:
  - MIT is permissive — you can redistribute MIT-licensed code alongside Apache code, but you must preserve the MIT copyright/notice.
  - However, you cannot *retroactively* force MIT-contributed code to become Apache 2.0 without the contributors’ permission.

- **Recommended practice**:
  - Audit source files for license headers and third-party material before copying.
  - Include a `THIRD_PARTY_NOTICES` or `NOTICE` file if you combine multiple origins.
  - Document license provenance in `docs/licensing.md`.

