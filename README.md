# rookiedred.github.io

Personal portfolio for Hung-Yu Lin — software engineer (iOS · Flutter · ML research).

**Live:** https://rookiedred.github.io/

## Stack

A hand-written static site. No framework, no build step, no dependencies. GitHub Pages
serves the repository root directly from `main`, so anything pushed there is live.

## Layout

```
index.html              single-page site; CSS and JS are inline
projects/
  ar-navigation.html    AR Navigation System
  atv.html              Autonomous Robot Car
  memory-vault.html     MemoryVault AI
  project-4.html        placeholder card
media/
  profiles/  logos/  projects/
Hung-Yu_Lin_Resume.pdf
```

`index.html` holds the whole homepage: hero, about, skills, experience, projects,
publication, education, contact. Its `:root` block defines the design tokens
(`--bg`, `--accent`, `--max-w`); the script at the bottom drives the scroll progress
bar, the `.reveal` fade-ins, nav highlighting, and the mobile menu.

Each project page is self-contained and carries its own inline stylesheet.

## Editing

Open the file and edit it — there is nothing to compile. To preview locally:

```bash
python3 -m http.server 8787
```

Then visit http://localhost:8787.

## Media conventions

Keeping the site fast depends on how media is added, so follow these rules:

- **Never commit a GIF.** Encode screen recordings and clips as H.264 MP4 and embed
  them with `<video loop muted playsinline preload="none" data-inview-play poster="…">`.
  The small script at the bottom of each project page starts playback only once the
  video scrolls into view, so nothing downloads until it is actually seen.

  ```bash
  ffmpeg -i in.gif -vf "scale=960:-2:flags=lanczos,hqdn3d=4:3:6:4" \
         -c:v libx264 -preset slow -crf 30 -pix_fmt yuv420p \
         -movflags +faststart -an out.mp4
  ffmpeg -i out.mp4 -frames:v 1 -q:v 5 out.jpg   # poster
  ```

- **Resize to the size it is displayed at**, times two for retina — not the size the
  camera or screenshot produced. A photo in a three-column strip needs ~700px, not 2556px.

- **Convert Display P3 to sRGB.** iPhone screenshots and photos are P3; copying the raw
  pixels into a file tagged sRGB makes colours oversaturated. `sips` does it correctly:

  ```bash
  sips -s format jpeg -s formatOptions 82 --resampleWidth 1000 \
       --matchTo "/System/Library/ColorSync/Profiles/sRGB Profile.icc" \
       in.PNG --out out.jpg
  ```

- **Always set `width` and `height`** on `<img>` and `<video>` so the layout does not
  shift while media loads, and add `loading="lazy" decoding="async"` to anything below
  the fold.
