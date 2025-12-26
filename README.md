# BinaryShrink

**Topic:** BinaryShrink: Browser-based File Compression Tool (Huffman + RLE)

A small, **lossless** compression and decompression tool that runs entirely in the browser (static site, GitHub Pages friendly).

It supports two algorithms:

- **Run-Length Encoding (RLE)**: simple and fast, best for data with repeated bytes.
- **Huffman Coding**: builds a prefix code from byte frequencies, often better for general text.

## How to run

### Option A: Open locally
1. Download/clone this repository
2. Open `index.html` in a modern browser (Chrome, Edge, Firefox)

### Option B: GitHub Pages
1. Push this repository to GitHub
2. Enable Pages for the repository (deploy from branch)
3. Open the published link and test compress and decompress

## How it works (high level)

- Read the selected file as bytes (`ArrayBuffer`)
- Compress using the chosen algorithm
- Save a single output file with a small header that stores:
  - magic bytes
  - algorithm used
  - original file name
  - original file size
  - (Huffman only) a 256-entry frequency table so decoding can rebuild the same tree

## Output format

The output file extension is **.bzc** (BinaryShrink Container).

Header layout (little-endian):

- 4 bytes: ASCII magic `BZC1`
- 1 byte : algorithm id (`1 = Huffman`, `2 = RLE`)
- 2 bytes: filename length `N` (uint16)
- N bytes: filename UTF-8
- 4 bytes: original size (uint32)
- If algorithm is Huffman:
  - 256 * 4 bytes: frequency table (uint32 each)

Payload:
- Huffman: packed bitstream
- RLE: pairs of (count, value) bytes

## Demo video (2 to 5 minutes)

Suggested flow:
1. Intro: what the tool is and what lossless compression means
2. Show UI: pick algorithm, compress a file, download output
3. Decompress the `.bzc` file back to the original
4. Mention key OS concepts: file I/O, buffering, binary formats, encoding/decoding

## Folder structure

- `index.html`, `styles.css`: UI
- `src/algorithms/*`: compression implementations
- `src/fileformat.js`: header encoding/decoding
- `tests/test.js`: Node-based correctness tests (optional)

## Node tests (optional)

If you want to run algorithm tests locally:

```bash
npm install
npm test
```

This only tests the pure byte algorithms, not the browser UI.


## Note on downloads
After you click Run, the app will show a Download button and also tries to auto-start the download. If your browser blocks auto-download, just click **Download result**.
