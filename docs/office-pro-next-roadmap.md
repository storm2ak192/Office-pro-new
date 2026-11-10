# Office Pro Next - Product Roadmap

## Project identity

Office Pro Next is an Android office suite being developed from the open-source
Kern project. Kern remains the upstream foundation; this repository is the
Office Pro product fork. Preserve upstream copyright and licence notices, and
keep this derivative project compliant with AGPL-3.0 and the licences of every
bundled dependency.

This document records the approved product direction. It is a roadmap, not a
claim that the listed features are already implemented.

## Product goals

- Native Android application using Kotlin and Jetpack Compose.
- Arabic-first capable interface with correct RTL layout and mixed Arabic /
  English document content, plus English UI.
- Word, Excel, PowerPoint, PDF and file-management experiences.
- Offline-first operation. Core editing must not depend on network access or a
  paid API.
- Optional AI assistant. It must remain disabled unless the user chooses to
  use it; documents must never be uploaded without a clear, explicit action.
- Responsive operation on Android 12 and resource-constrained phones and tablets.
- Reliable save, recovery and preservation of original documents.

## Approved implementation sequence

1. Establish the Office Pro Next project identity and inspect the inherited
   architecture, dependencies, licences and CI.
2. Strengthen shared foundations: file access, safe saving, backups, recovery,
   memory bounds, background processing and document-session lifecycle.
3. Develop the existing Kern PDF editor rather than replacing it.
4. Develop Word editing and round-trip fidelity.
5. Develop Excel editing, formulas and workbook fidelity.
6. Develop PowerPoint editing and presentation fidelity.
7. Port useful Office Pro features selectively, after review and re-engineering.
8. Add an optional AI assistant without making core features depend on it.
9. Expand automated format, regression, UI and performance testing.
10. Prepare a release only after the implementation and tests are reviewed.

## PDF requirements

Retain and extend the existing Kern PDF viewer and editing foundation. Planned
work includes:

- Android 12-compatible text search and selection.
- Annotations: highlight, freehand drawing, text notes, comments and signatures.
- Page operations: rotate, reorder, remove, extract, split and merge.
- Image insertion and PDF export.
- Password-protected PDFs: detect encryption, request the opening password,
  distinguish user/open permissions from owner permissions, and expose only
  operations allowed by the document's effective permissions.
- Preserve encryption and password protection on save where technically
  supported. Never silently strip protection.
- Warn that changing a digitally signed PDF can invalidate its signature.
- Distinguish editing original text, placing a visual overlay, and secure
  redaction. Do not describe an overlay as secure redaction.
- Investigate Arabic OCR and genuine text editing as separate capabilities;
  do not claim either is available until verified.

## Engineering constraints

- Do not overwrite the only copy of a document before a replacement has been
  written and validated.
- Prefer Android Storage Access Framework and user-granted URI access. Avoid
  broad storage permissions unless a feature demonstrably requires them.
- Keep large document processing off the main thread and bound bitmap, cache
  and in-memory document sizes.
- Handle malformed and unusually large files without crashing or exhausting
  device memory.
- Keep the application usable on Android 12; do not rely exclusively on newer
  framework APIs.
- Keep cloud AI and all network-dependent features optional and separately
  consented to.
- Do not claim tests or builds passed unless they were actually run and their
  results inspected.

## Current inherited baseline

The current repository is based on Kern 0.1.14.3. The inherited Word, Excel
and PowerPoint editors use Apache POI. The current PDF screen is primarily a
viewer; its annotation toolbar is not yet functional, and its native bridge
currently provides limited page operations. PDF text extraction and selection
depend on Android 15 framework APIs in the inherited implementation, so this
does not meet the Android 12 target yet.

The inherited file I/O reads complete documents into byte arrays and writes
directly to the destination. This is a high-priority reliability and memory
risk to address before expanding editing features.

## Status convention

A roadmap item is considered complete only after implementation, appropriate
automated tests, and review of the resulting behavior. A successful APK build
alone does not prove document fidelity or safe saving.
