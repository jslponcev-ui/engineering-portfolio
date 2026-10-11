# Add a typed master-MAC device query and validate response envelopes

Home Assistant Core PR https://github.com/home-assistant/core/pull/185565 preserves the legacy `format=master_mac|hostname|is_online` query. The existing `get_detected_devices()` uses the separate `mac` field and derives presence from `last_seen` when the online flag is missing, so it cannot replace that query without changing identity or presence behavior.

Add `get_master_mac_devices() -> list[MasterMacDevice]` with the exact query, uppercase `master_mac` identity, optional hostname, and the existing boolean online flag semantics. Missing online flags remain offline. Missing, non-string or empty master MACs are skipped; malformed response lists raise `FortiOSResponseError` rather than appearing as an empty successful scan. A valid empty list remains valid. Neither `mac` nor `last_seen` is used as a fallback.

Validate the envelopes used by this method and `get_system_status()`, including embedded HTTP 401/403 responses mapped to authentication errors. Export the model and document the API under Unreleased. Existing `get_detected_devices()` behavior and caller ownership of the aiohttp session are preserved.

Validation of local commit `707f2495f2609c7c1a8d5213142fbcefda45993c` on Python 3.14.5: 52 tests passed, all 201 statements covered, aggregate statement/branch coverage 99% (one pre-existing uncovered branch); Mypy, Ruff, formatting, source/wheel build and Twine metadata checks passed. Four warnings come from aresponses using deprecated asyncio.iscoroutinefunction. Python 3.12/3.13 CI has not been run locally. A dependent Core adapter passed 59 tests with 100% coverage against this proposed source.

The version is intentionally unchanged: these are proposed Unreleased changes, not a new PyPI release. After maintainer review and merge, a published version containing both the typed master-MAC method and status-envelope validation is required before Core can switch to them and pin the real release. No hardware validation is claimed.
