# Bulk Image Grabber

A single-file HTML/CSS/JavaScript tool that lets you paste a webpage URL, view all the images on that page, select the ones you want, and download them together as a `.zip` file.

## Files

- `bulk-image-downloader.html` — the entire tool. No installation, no build step, no server required.

## How to run it

This must be opened as a **standalone browser tab**, not previewed inside a sandboxed viewer (e.g. Claude's in-chat preview), because it needs to reach external servers.

1. Download `bulk-image-downloader.html` to your computer.
2. Double-click it (or right-click → Open with → your browser).
3. Confirm the address bar shows something starting with `file:///`.

## How to use it

1. Paste a page URL into the input box (e.g. `https://example.com/gallery`).
2. Click **Load Images**. The tool fetches the page's HTML and extracts every image it can find.
3. Click individual thumbnails to select them, or check **Select all**.
4. Click **Download selected (.zip)**. Your browser will download a single `images.zip` containing all selected images.
5. Extract the zip anywhere on your computer.

## How it works

- **Fetching the page:** Browsers block JavaScript from reading another site's raw HTML directly (CORS policy). To get around this, the tool routes the request through a public CORS proxy. It tries three different proxies in order (`allorigins.win`, `corsproxy.io`, `codetabs.com`) so that one being down doesn't fully block you.
- **Finding images:** It scans the fetched HTML for `<img>` tags (`src`, `data-src`, `data-lazy-src`, and the first entry in `srcset`) as well as CSS `background-image` values on inline styles, then converts all relative URLs to absolute ones.
- **Downloading:** Each selected image is fetched individually and packed into a `.zip` using [JSZip](https://stuk.github.io/jszip/), loaded from a CDN. The browser then triggers its normal save dialog for the finished zip.

## Limitations

- **JavaScript-rendered pages:** Sites that load images dynamically via JavaScript (many single-page apps, infinite-scroll galleries) may show few or no results, since the proxy only sees the initial HTML — not what loads afterward.
- **Blocked downloads:** Some image hosts don't allow cross-origin fetching of their files. Those thumbnails will appear dimmed and marked "failed" — click one to open it directly in a new tab instead.
- **Proxy reliability:** The CORS proxies are free public services and can occasionally be slow, rate-limited, or temporarily down. If loading fails, wait a moment and try again.
- **Copyright:** This tool doesn't check image licensing. Make sure you have the right to download and reuse any images you save.

## Possible upgrade

For more reliable results on JavaScript-heavy sites (no proxy dependency, reads the live rendered DOM), this could be rebuilt as a browser extension. Ask if you'd like that version.
