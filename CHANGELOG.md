# Changelog

## v1.4.37

- **Date formatting** — all dates now respect the user's YouTrack profile settings: timezone, date format pattern, 12/24h preference, and UI language; fetched once via REST API and cached for the session
- **Cleanup** — removed dead code: unused handlers, stale imports, deprecated helpers, orphaned utility module
- **Settings** — renamed connection string key with backward-compatible fallback; clarified that stored credentials are obfuscated, not encrypted, and readable by instance admins (per JetBrains Marketplace review feedback, see JT-88186); updated UI labels and documentation

## v1.4.34

- **Security** — removed insecure endpoint, hardened delete handler (server-side clock skew only)
- **Security** — added empty prefix guard in presigned URL generation
- **Performance** — S3 thumbnails no longer re-fetched on a timer; once loaded, they stay in browser cache permanently. Expired presigned URLs are retried on demand (only on `<img>` error), eliminating periodic thumbnail traffic entirely
- **Performance** — backend settings cached (30s TTL + inflight dedup) instead of per-thumbnail HTTP request
- **Performance** — concurrent presign requests for the same file key deduplicated via inflight guard
- **Performance** — React state updates skipped when nothing actually changes in thumb invalidation
- **Performance** — metadata write skipped on auto-refresh when counters unchanged (saves 3–5 HTTP requests/min in steady state); `storageId` fetch cached from meta
- **Performance** — deduplicated presign setup and backend settings loading across all call sites
- **Preview** — XLS and XLSB support via full SheetJS library (bundled); mini build used for XLSX
- **Preview** — text-based file thumbnails rendered as canvas previews (code, configs, logs, CSV, XML, etc.)
- **Reliability** — in-browser mutex serializes all meta read-modify-write cycles, preventing lost updates when upload and auto-refresh overlap
- **Native attachments** — upload/delete now logged to issue comments (same as S3), unified comment format
- **Cleanup** — removed all debug infrastructure and unused backend handlers from production bundle

## v1.3.476

- **Encryption hardening** — per-installation dynamic key (`a3x:` format) with backward-compatible fallback
- **Meta storage in YouTrack** — per-issue metadata moved to extension properties; bidirectional S3 ↔ YT lazy migration; `metaStorage` setting (`youtrack` / `s3`)
- **Document previews** — bundled local libs (no CDN); XLSX multi-sheet tabs; odf-kit replaces webodf (AGPL → Apache-2.0)
- **UI/UX improvements** — dark theme for thumbnails and previews; list view column alignment; typed thumbnail backgrounds
- **Build & backend cleanup** — debug files excluded from production; deduplicated code, dead code removal

## v1.3.400

- **Unified File View** — browse native YouTrack attachments and S3 files in one widget
- **Preview Without Download** — preview native attachments directly
- **Full native attachment management** — list, preview, download and delete
- **Progressive loading** — non-blocking UI, inline loaders, parallel fetch
- **JSON preview** — syntax-highlighted, auto-formatted
- **Markdown preview** — rendered with proper formatting
- **ZIP preview** — browse archive contents without downloading
- **Colored badges** — file-type badges with distinct colors
- **Renamed** — widget renamed to Adv.Attachments
- **Widget permissions** — added CREATE_ATTACHMENT_ISSUE, DELETE_ATTACHMENT_ISSUE

## v1.2.300

- **Configurable file size limits** — min/max upload sizes in MB with user-friendly messages
- **File operation comments** — automatic issue comments on upload/delete with grouping
- **Comment visibility control** — optional restriction to specific user groups
- **Backend minification** — all backend JS minified with Terser for smaller package

## v1.2.x

- **No credentials leak** — all sensitive data redacted from logs regardless of level
- **Auto clock skew detection** — transparent time sync between YouTrack and S3
- **Zero-flicker auto-refresh** — silent background updates every 60 seconds
- **Persistent thumbnails** — image previews preserved between refreshes
- **Lazy loading** — images load on-demand as you scroll
- **Office files preview** — XLSX, DOCX, ODT preview directly in YouTrack
- **Reorganized settings** — logical groups for S3, TTL, Widget UI
- **Modular backend** — refactored into organized modules

## v1.0.x

- **Initial release** — S3-compatible storage integration for YouTrack
- **Grid and list views** — thumbnails or compact list with auto-switching
- **Direct file operations** — upload, download, delete via presigned URLs
- **Security** — least-privilege permission model (READ_ISSUE, UPDATE_ISSUE, ADMIN)
- **Path normalization** — unified key building with base directory support
- **Centralized presign logic** — all presign URLs and anti-cache handling unified
