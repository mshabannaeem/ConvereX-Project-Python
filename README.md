# ConvereX

ConvereX is a set of online converters in one website: QR codes, Morse code, binary, ASCII, and file conversion for documents, images, audio, video, archives, scripts and data files.

The backend is written in Python (Flask) using object-oriented programming. The frontend is a single HTML page styled with Tailwind CSS and one small CSS file, with plain JavaScript for the browser features. The code is commented throughout and kept at a beginner-friendly level, so it also works as a learning project.

<p align="center">
  <img src="docs/screenshots/home-light.png" alt="ConvereX home page in the light theme" width="49%">
  <img src="docs/screenshots/home-dark.png" alt="ConvereX home page in the dark theme" width="49%">
</p>
<p align="center">
  <img src="docs/screenshots/converter-page.png" alt="A converter page with its list of conversions and an upload area" width="62%">
  <img src="docs/screenshots/mobile-home.png" alt="ConvereX home page on a phone" width="24%">
</p>

## Contents

- [Features](#features)
- [Converters](#converters)
- [Tech stack](#tech-stack)
- [Getting started](#getting-started)
- [Programs you also need](#programs-you-also-need)
- [Project structure](#project-structure)
- [How the Python code is organised](#how-the-python-code-is-organised)
- [API routes](#api-routes)
- [Configuration and customising](#configuration-and-customising)
- [Conversion limits you should know about](#conversion-limits-you-should-know-about)
- [Security and privacy](#security-and-privacy)
- [Testing status](#testing-status)
- [License](#license)

## Features

**Website**

- Clean, responsive layout for phones, tablets and desktops, with no horizontal scrolling
- Light theme (white, cream, green, brown, light orange) and dark theme (black, charcoal, gray, light gray), with a toggle that remembers your choice
- Sticky header with a navigation menu, an active-page highlight and bubble hover effects
- Scrolling sidebar listing every converter, and a mobile menu for small screens
- One page per converter category, listing every conversion it supports, with deep links such as `#/image/jpg-png`
- Site search that finds categories and individual conversions (try "pdf" or "jpg png")
- Feedback form and reviews section. Only reviews that visitors really submit are shown
- Privacy policy page, simple cookie notice, custom 404 page, back-to-top button
- Success and error popup messages, and an offline notice
- Favicon, page titles and meta descriptions, clickable logo, copyright year that updates itself

**Browser features (`frontend/APIs.js`)**

- Web Storage API: `localStorage` (theme, cookie notice, recently used converters) and `sessionStorage` (drafts of text you typed)
- Fetch API: GET and POST requests to the Python server
- Clipboard API: copy results and paste input
- Notification API: tells you when a conversion has finished
- Online/Offline detection

## Converters

| # | Category | What it does | File |
|---|----------|--------------|------|
| 1 | QR Code Generator | Text or link to a PNG QR code | `QR_Code_Generator.py` |
| 2 | Morse Code | Text to Morse, Morse to text | `Morse_Code.py` |
| 3 | Binary Converter | Digital (text) to binary, binary to digital, decimal to binary, binary to decimal | `Binary_Converter.py` |
| 4 | ASCII Value Generator | ASCII to value, value to ASCII | `ASCII_Value_Generator.py` |
| 5 | Text Converter | TXT, DOCX, PDF, RTF, ODT in any direction (20 conversions) | `Text_Converter.py` |
| 6 | Image Converter | JPG, PNG, GIF, BMP, SVG, PSD in any direction (30) | `Image_Converter.py` |
| 7 | Audio Converter | MP3, WAV, AAC in any direction (6) | `Audio_Converter.py` |
| 8 | Video Converter | MP4, MKV, AVI, MOV in any direction (12) | `Video_Converter.py` |
| 9 | Archive Converter | ZIP, RAR, 7Z, TAR, GZ in any direction (20) | `Archive_Converter.py` |
| 10 | File Converter | PY, BAT, SH, JS, C to the other languages or to EXE (25) | `File_Converter.py` |
| 11 | Program/Data Converter | XLSX, CSV, SQL, MDB, ACCDB in any direction (20) | `Program_Data_Converter.py` |

## Tech stack

- **Backend:** Python 3.10 or newer (developed on 3.12), Flask
- **Frontend:** HTML, Tailwind CSS (Play CDN), custom CSS, vanilla JavaScript
- **Fonts:** FlashPro Text for headings, Geist Mono for descriptions
- **Python libraries:** qrcode, Pillow, cairosvg, psd-tools, vtracer, python-docx, reportlab, pypdf, striprtf, odfpy, py7zr, rarfile, openpyxl, PyInstaller
- **Outside programs:** FFmpeg and a few optional tools (see below)

## Getting started

```bash
git clone <your-repository-url>
cd ConvereX

python -m venv .venv
# Windows:      .venv\Scripts\activate
# macOS/Linux:  source .venv/bin/activate

pip install -r requirements.txt
python app.py
```

Open http://127.0.0.1:5000 in your browser.

A converter whose Python package or outside program is missing does not crash the site. It shows a clear message in an error popup that says what to install.

## Programs you also need

| Needed for | Install |
|------------|---------|
| Audio and Video converters | [FFmpeg](https://ffmpeg.org), with `ffmpeg` available on your PATH |
| SVG input in the Image Converter | The Cairo graphics library (on Windows, the GTK runtime) |
| Reading `.rar` files | `unrar` or `bsdtar` |
| Creating `.rar` files | WinRAR's command-line `rar` tool (RAR is a closed format) |
| `.c` to `.exe` | MinGW-w64 gcc (`x86_64-w64-mingw32-gcc`), or gcc on Windows |
| Reading `.mdb` / `.accdb` | mdbtools (Linux/macOS), or the Microsoft Access Database Engine with `pyodbc` (Windows) |
| Creating `.mdb` / `.accdb` | Windows only: Access Database Engine, `pyodbc` and `pywin32` |

## Project structure

```
ConvereX/
├── app.py                        Flask server: routes, feedback storage, error handling
├── requirements.txt
├── backend/
│   ├── base_converter.py         Shared parent classes and helpers
│   ├── QR_Code_Generator.py
│   ├── Morse_Code.py
│   ├── Binary_Converter.py
│   ├── ASCII_Value_Generator.py
│   ├── Text_Converter.py
│   ├── Image_Converter.py
│   ├── Audio_Converter.py
│   ├── Video_Converter.py
│   ├── Archive_Converter.py
│   ├── File_Converter.py
│   └── Program_Data_Converter.py
├── frontend/
│   ├── index.html                The single page; every site page lives inside it
│   ├── style.css                 Theme colours, components, animations
│   ├── APIs.js                   Browser feature programs (storage, fetch, clipboard, ...)
│   ├── Addons.js                 Popups, theme toggle, menu, cookie banner, search, ...
│   └── main.js                   Page building, routing, converter panels, feedback
├── Visuals/                      Logos, favicon, monogram, hero images
├── docs/screenshots/             Images used in this README
└── data/                         reviews.json is created here when feedback is sent
```

## How the Python code is organised

All four object-oriented ideas are used, and each one is explained in comments inside `backend/base_converter.py`.

```
BaseConverter (abstract)
├── TextTool                  turns text into text
│   ├── MorseCode
│   ├── BinaryConverter
│   └── ASCIIValueGenerator
├── FileConverter             turns one file format into another
│   ├── TextConverter
│   ├── ImageConverter
│   ├── AudioConverter        (also inherits FFmpegMixin)
│   ├── VideoConverter        (also inherits FFmpegMixin)
│   ├── ArchiveConverter
│   ├── ScriptConverter       (File_Converter.py)
│   └── DataConverter         (Program_Data_Converter.py)
└── QRCodeGenerator
```

- **Abstraction:** `BaseConverter` declares what every converter must do and leaves the details to the children.
- **Encapsulation:** data such as `_slug`, `_name` and `_pairs` is private and reached through properties and methods.
- **Inheritance:** `TextTool` and `FileConverter` pass shared behaviour (checking input, building the list of conversions) down to every category.
- **Polymorphism:** `app.py` calls `describe()`, `transform()` and `convert()` on any converter without caring which kind it is. Each one does the work its own way.

Most file converters follow the same two steps: read the source into a common form (text, an image object, a table, a folder of files), then write it out in the target format. That avoids writing one program for every pair.

## API routes

The JavaScript calls these with the Fetch API.

| Method | Route | Purpose |
|--------|-------|---------|
| GET | `/api/categories` | Lists every converter and the conversions it supports |
| POST | `/api/text/<slug>` | Morse, binary and ASCII. JSON `{mode, text}` in, `{result}` out |
| POST | `/api/qr` | JSON `{text}` in, PNG image out |
| POST | `/api/convert/<slug>` | Form data `file` and `target` in, converted file out |
| GET | `/api/reviews` | Approved reviews, newest first |
| POST | `/api/feedback` | JSON `{name, rating, message}` in |

Errors always come back as JSON `{"error": "message"}`, which the site shows in a popup.

## Configuration and customising

**Settings in `app.py`**

| Setting | Default | Meaning |
|---------|---------|---------|
| `MAX_UPLOAD_MB` | `200` | Largest upload accepted. Keep `MAX_UPLOAD_MB` in `frontend/main.js` the same |
| `AUTO_APPROVE_REVIEWS` | `True` | Set to `False` to approve reviews yourself by editing `"approved"` in `data/reviews.json` |
| `ENABLE_SCRIPT_CONVERTER` | on | Environment variable. Start with `ENABLE_SCRIPT_CONVERTER=0` to switch the File Converter off |
| `FLASK_DEBUG` | off | Environment variable. Set to `1` for local development only |

**Logos and name**

Replace the files in `Visuals/` and keep the same names: `logo-primary-light.svg`, `logo-primary-dark.svg`, `logo-secondary-light.svg`, `logo-secondary-dark.svg`, `monogram.svg` and `favicon.svg`. If you use PNG files instead, update the `src` attributes in `frontend/index.html`. The site name appears in `index.html`, in `SITE_NAME` in `main.js`, and in the privacy policy.

**Fonts**

FlashPro Text is not available from Google Fonts. Put its `.woff2` file in `Visuals/fonts/` and follow the comment at the top of `frontend/style.css`. Until then the headings use Georgia. Geist Mono loads from Google Fonts.

**Tailwind**

`index.html` loads Tailwind from its CDN, which is convenient while developing. For a public launch, switch to the Tailwind CLI build so the site does not depend on the CDN.

## Conversion limits you should know about

- **Text Converter:** only the text is converted. Fonts, colours, tables and pictures are not kept. PDF input must contain real text, because scanned PDFs need OCR first. PDF output uses a standard Latin font.
- **Image Converter:** converting to SVG traces the picture into vector shapes, so the result is an approximation. Converting to PSD gives one flat layer. Animated GIFs use only their first frame.
- **Audio and Video:** files are re-encoded with FFmpeg. Converting between lossy formats loses a little quality, and long videos can take minutes.
- **Archive Converter:** a `.gz` holds one file, so archives with several files become `.tar.gz`.
- **File Converter:** these are not true format conversions.
  - To `.exe` means packaging or compiling. `.c` is compiled with gcc, and `.py` is packaged with PyInstaller (the server must run on Windows). `.bat`, `.sh` and `.js` are packaged inside a small Python launcher, so the interpreter must exist on the PC that runs the `.exe`.
  - Between scripting languages, the new file wraps your original code and runs it. It is not translated line by line.
  - `.exe` is only a target, because turning an executable back into source code needs decompiling, which is unreliable.
- **Program/Data Converter:** only the first sheet or table is converted. `.sql` input must be SQLite-style SQL. Creating `.mdb` and `.accdb` files works only on Windows with the Access Database Engine.

## Security and privacy

- Uploaded files are saved in a temporary folder only while they are converted and are deleted straight afterwards.
- External programs are started with argument lists, never through a shell, so file names cannot inject commands. Archive paths are checked to block "zip slip" attacks, and uploaded SQL is run in a throw-away in-memory database that blocks `ATTACH`, `DETACH` and `PRAGMA`.
- Feedback text is displayed as plain text, never as HTML, and the form has a hidden spam-trap field.
- The **File Converter compiles and packages code that visitors upload**. Run it only on your own computer or behind a login, or switch it off with `ENABLE_SCRIPT_CONVERTER=0`.
- Do not use Flask's built-in server or debug mode on a public site. Use a production server such as gunicorn or waitress.
- The privacy policy page describes what this code does. Review it against the laws that apply to you, and update it if you add analytics, self-host fonts or change what is stored.

## Testing status

Tested: Morse, binary and ASCII, and the Text (except RTF reading), Image (JPG, PNG, GIF, BMP), Audio, Video, Archive (ZIP, TAR, GZ), script wrapper (PY, SH, JS, C targets) and data (CSV, XLSX, SQL) conversions, all API routes, and the whole website in headless Chromium at desktop and phone sizes in both themes (navigation, search, popups, clipboard, storage, feedback, 404, cookie banner, offline notice).

Not yet verified: QR code generation, RTF reading, SVG and PSD handling, 7Z and RAR, the generated `.bat` wrappers on Windows, PyInstaller `.exe` builds, and reading or creating `.mdb` / `.accdb` files. The code for these is written but needs a run on a machine that has the required packages or Windows tools.

## License

This project does not have a license yet. Add a `LICENSE` file (MIT is a common choice) to say how others may use the code.
