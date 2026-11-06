# Locale Check

<img src="src/assets/icon.svg" width="88" alt="Locale Check">

Compare JSON translation keys and placeholders. Local JavaScript / Node.js utility, version 0.1.0.

## Quick start

Node.js 22 or newer is required. On Windows, run Start.cmd. On macOS/Linux, run sh start.sh. Open http://127.0.0.1:8835/. Override the port with the PORT environment variable. The server listens only on 127.0.0.1 and serves an explicit file allowlist. Open the HTTP address rather than index.html directly: ES modules require HTTP.

Choose Example, then Run, review the result and save the files using the export links. For file-based tasks, select files you are permitted to process.

## Inputs

- **Source JSON** (textarea).
- **Translation JSON** (textarea).

Input, processing and files stay in the browser. Saving input on this device requires an explicit button action; selected files are not included in that legacy input save. Clear saved data removes only this application's saved input. Local storage is not encrypted: avoid shared browser profiles for sensitive data. Review exported results before using them in a production workflow.

## Scope and limitations

This does not assess the semantic accuracy of a translation. There is no network publishing, shared-user synchronization or automatic paid API access. Native macOS/Linux operation has not been verified on actual devices.

## Verification

node --test tests/*.test.mjs

The shared interface and server originate from product-launchpad/scaffold. This project is standalone and does not import neighboring applications. Tool logic is in src/js/tool.mjs. Included examples use synthetic data.

## License

Original code is licensed under MIT. Author: Denis Gutsuliak (d9911).

## Interface and local projects — 2026-11-04

The interface, fields, application errors and generated labels support Russian,
English and Spanish. User content is not translated. First opening follows browser
language and system appearance; selected language/theme persist in localStorage.
The sidebar is 250 px wide; narrow screens use a menu and tables scroll internally.

Save project stores one snapshot in IndexedDB. Open saved project restores it
explicitly. Select the file checkbox to include chosen files; otherwise reselect
them next time. Delete saved project removes the snapshot and legacy saved input,
while retaining the current form and preferences. Documents are not autosaved.

Storage estimates and a persistent-storage request are available. The browser may
deny persistence; data belongs to this origin, port and browser profile. Clearing
site data can remove it. Keep exported files and an independent backup.

A manifest and service worker support offline operation after a complete first
load. Supported browsers can install the PWA; native OS installation is unverified.
Updates never reload the page automatically: save your project before accepting an
update. Restart an existing server normally to load server changes, then refresh.

Distinct SVG, PNG 192/512 and Windows ICO icons identify this tool. No SQLite or
cloud dependency is needed. All runtime modules are local to this project:
i18n.mjs/messages.mjs, preferences.mjs, drafts.mjs, pwa.mjs and sw.js.
