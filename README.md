# fappr

A privacy‑first media browser and collage builder that runs entirely in your browser. Import photos and videos from a local folder or pick files via Dropbox Chooser, browse with fast filters and sorting, and assemble multi‑slot video/photo collages that play inline.

## Highlights

- Local‑only by default: no uploads; everything stays on your machine
- Dropbox Chooser import (optional): pick files from your Dropbox account (dotfiles hidden)
- Smart library UI
  - Filters: All / Photos / Videos
  - Sorting: A→Z (folders first), Date (grouped Today/Yesterday/Week/Month), Size (folders on top, files by size)
  - Breadcrumbs, search, recursive view, selection mode
  - Mobile‑first: two‑column thumbs on phones; buttons sized for touch
  - Unsupported formats (e.g., .wmv, .avi, .mkv) are badged and listed in a separate “Unsupported Videos” section
- Cover art and hiding
  - Hidden thumbnails: supply crisp cover art as `.<video-basename>.jpg`; folder cover as `.<folder-name>.jpg`
  - Non‑hidden “cover” images are auto‑paired and hidden: `-cover`, `_cover`, ` cover`, `-thumbnail`, `_thumb`, `-poster`, `_poster`, ` poster`, `-folder`, `_folder`, ` folder`
  - Applies to local folders and Dropbox picks
- Media viewer
  - Fullscreen photo/video with next/prev/escape and volume indicator
  - Mobile gestures: swipe left/right on photos (prev/next); swipe up/down on videos (volume), left/right (scrub)
  - Filename badge: smaller pill with auto‑hide after inactivity
  - Picture‑in‑Picture overlays (PIP): up to 4 photo/video overlays, with native video controls, loop, muted by default; move to any corner; automatic corner placement and swapping
- Collage builder (desktop‑only)
  - 20+ predefined layouts (grids, thirds, staggered, etc.)
  - Per‑slot photo/video; videos loop and start muted
  - Pan controls (D‑pad) to adjust visible portion in a slot
  - Toolbar controls: Play All, Pause All, Mute/Unmute All, Full Screen (auto‑hide after inactivity)
  - Favorites integration: pick from your Favorites “virtual folder” when filling slots

## Favorites

- Star any photo/video to add to Favorites (top‑right star on each tile)
- Open Favorites via the star icon in the top toolbar; clear all with the “Clear Favorites” button
- Favorites persist in localStorage, including persisted thumbnails (photos and generated video posters)
- Breadcrumb shows Root / Favorites; clicking Root exits the Favorites view

## Quick start

This is a single‑file React app (in‑browser Babel + Tailwind CDNs). No build step is required.

1) Serve the folder locally (required for Dropbox Chooser and best browser APIs):

- Python: `python3 -m http.server 3000`
- Node: `npx serve -l 3000`

Open http://localhost:3000 and use the landing page buttons:

- Choose Folder: import a local folder (webkitdirectory)
- Last Folder: re‑open the previously chosen folder (local/Chromium only)
- Dropbox: pick images/videos via Dropbox Chooser (see setup below). On mobile, the Dropbox button is centered.

Note: File System Access APIs (for Last Folder) work in Chromium‑based browsers and when running locally. Safari/Firefox still support the folder `<input>` method.

## Dropbox Chooser setup (optional)

fappr integrates Dropbox “Drop‑ins” Chooser — no server or secrets required.

- App key: set in `index.html` Drop‑ins script tag
- Allowed domains: in your Dropbox App Console, add `localhost` (for dev) and your production domain
- Serve over HTTP(S): Chooser will not load on `file://`

What the app does with Chooser results:

- Photos: use the direct content link for thumbnails and display (crisp)
- Videos: use the direct content link for playback; hidden `.jpg` next to the video or a non‑hidden “cover” image will be used for the thumbnail when selected together
- Dotfiles (`.*`) are ignored, and “folder selection” is not used (Chooser limitation)

If you need real folder browsing or Dropbox thumbnails for videos, switch to the full Dropbox API with OAuth (PKCE) or a small serverless proxy.

## Privacy

- Local imports: nothing leaves your computer
- Dropbox Chooser: authentication and file delivery happen via Dropbox; the app reads the links returned by Dropbox and does not upload your files elsewhere

## Usage tips

- Hidden thumbnails
  - Video cover art: `.<basename>.jpg` (e.g., `myclip.mp4` → `.myclip.jpg` in the same folder)
  - Folder cover: `.<folder-name>.jpg` inside that folder
- Non‑hidden cover art
  - Name like `video-cover.jpg`, `video-thumbnail.jpg`, `video-poster.jpg`, or `video-folder.jpg`
  - Matching covers are used for video/folder thumbnails and hidden from the grid
- Date view: folders are included and grouped by the latest modified time of their contents
- Unsupported videos: always shown at the bottom in a dedicated section; still visible in the main grid if you prefer

## UI details

- Toolbars: unified blue icon buttons for Select and Full Screen; Upload is an icon; Collage button sits leftmost
- Landing: subtle static styling (no animated shimmer), purple “Download Website” button, short privacy note
- Thumbnails: mobile shows two columns; exiting fullscreen restores the normal column count

## Browser support

- Recommended: latest Chrome/Edge (Chromium)
- Works in Safari/Firefox with folder input; “Last Folder” and some APIs may be limited

## Development

- Single file: `index.html` contains React components and styles
- No build tools needed; edit and refresh

## Troubleshooting

- Dropbox Chooser won’t open
  - Ensure the Drop‑ins script tag has your app key
  - Add `localhost` and your production domain in the Dropbox app’s allowed domains
  - Serve over HTTP(S), not `file://`
- Blurry thumbnails
  - For images, the app uses the original content link for crisp thumbnails
  - For videos, provide hidden `.jpg` cover art or adopt the full Dropbox API for generated thumbnails
- “Last Folder” missing
  - It’s hidden on non‑local origins by design; visible on `file://`, `localhost`, or `127.0.0.1`

## Security

- Do not embed Dropbox app secrets or access tokens in client code
- The Drop‑ins app key is public by design and should be restricted to your domains in the Dropbox console

---

If you want a hosted guide, PKCE OAuth flow, or serverless proxy to browse Dropbox folders, open an issue and we’ll outline the minimal steps.
