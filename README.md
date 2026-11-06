# Locale Check

<img src="src/assets/icon.svg" width="88" alt="Locale Check">

Find missing translation keys, unexpected keys and placeholder mismatches in two
JSON documents. Locale Check runs locally in your browser and exports a JSON
report you can review before updating your translation files.

## Quick start

Install Node.js 22 or newer, then run this command from the project directory:

```sh
node server.mjs
```

Open **http://127.0.0.1:8835/**. Stop the server with **Ctrl+C**.

| Platform | Launcher |
| --- | --- |
| Windows | `Start.cmd` or `Start.bat` |
| macOS | `sh Start.command` |
| Linux | `sh start.sh` |

The server binds to `127.0.0.1` and serves an explicit list of application files.
Use the HTTP address rather than opening `index.html` directly: browser modules
require HTTP. The `PORT` environment variable overrides the default port.
Native macOS and Linux operation has not been verified on actual devices.

## Check a translation

1. Select English in the interface if your browser selected another language.
2. Paste the reference document into **Source JSON**.
3. Paste the document to check into **Translation JSON**.
4. Choose **Run**, inspect the issue table and download `locale-report.json`.

The **Example** button supplies synthetic demonstration data. It does not load
your files or connect to a translation service.

## Example

Source JSON:

```json
{
  "greeting": "Hello {name}",
  "nav": {
    "home": "Home"
  }
}
```

Translation JSON:

```json
{
  "greeting": "Welcome {user}",
  "extra": "Unused entry"
}
```

With English selected, the result is **Source keys: 2; issues: 3**:

| Key | Issue |
| --- | --- |
| `greeting` | Placeholder mismatch |
| `nav.home` | Missing translation |
| `extra` | Extra key |

The downloaded report contains the `sourceKeys` count and an `issues` array with
the same key-and-issue pairs.

## Supported input

- Both JSON roots must be objects. Nested objects are flattened into paths such
  as `nav.home`.
- Leaf values must be strings. Arrays, numbers, booleans and null values are
  rejected. Literal dots in key names are not supported.
- Named placeholders include `{name}` and `{{name}}`. Format placeholders
  include `%s`, `%d`, `%i`, `%f` and positional forms such as `%1$s`.
- Placeholder order does not matter; placeholder names, formats and repetition
  counts do. A missing or extra occurrence produces a mismatch.
- ICU `plural`, `select` and `selectordinal` expressions are rejected. This is
  not a full ICU or printf parser.

The tool checks structure and placeholders. It does not judge translation
quality, meaning, spelling or grammar, and it does not translate text.

## Data and saved projects

The JSON documents and comparison run in the browser. The local server does not
receive your translation content. Downloads are saved through your browser.

**Save project** explicitly stores one snapshot in IndexedDB. **Open saved
project** restores it. **Delete saved project** removes the saved snapshot and
legacy saved input while keeping the current form and interface preferences.
Documents are not autosaved.

Saved data belongs to this origin, port and browser profile. Browser storage is
not an encrypted vault; clearing site data may remove it. Keep exported files and
an independent backup. A persistent-storage request may be declined by the browser.

The interface supports English, Russian and Spanish. First use selects the first
supported entry in the browser's language preferences (including regional variants
such as `es-MX` and `ru-RU`); if none match, it uses English. A valid saved manual
language choice takes priority. If browser storage is unavailable, automatic
selection still works for the current visit. System appearance is followed;
explicit language and theme selections are saved
locally. User-provided text is never translated.

## Offline use

After a complete first load, the service worker can serve the cached application
offline. Compatible browsers can install it as a PWA; native OS installation has
not been verified. Updates do not reload the page automatically: save your work
before accepting an update. Restart the server normally after server changes.

There is no cloud synchronization, shared-user authentication, automatic
publishing or paid API access.

## Development and verification

```sh
node --test tests/*.test.mjs
```

Tests cover missing and extra keys, placeholder order and repetition, unsupported
values and rejected ICU expressions.

- [Comparison logic](src/js/tool.mjs)
- [Local HTTP server](server.mjs)
- [Tests](tests/tool.test.mjs)

The shared interface originated in product-launchpad/scaffold. This repository
is standalone and does not import neighboring applications. Its styles, scripts,
icons and browser-storage modules are included locally.

## License and translations

Original code: **Denis Gutsuliak (d9911)**. Licensed under [MIT](LICENSE).

The primary README is English. Separate translations are available in
[Russian](README.ru.md) and [Spanish](README.es.md).
