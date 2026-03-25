# canvas-scroll-sequence

> Scroll-driven canvas frame sequencer — 150 WebP frames painted via the Canvas 2D API in sync with scroll position, replicating Apple's iconic image-sequence technique.

---

## What this demonstrates

Apple's product pages animate objects by rapidly cycling through hundreds of still-image frames as the user scrolls — creating the illusion of smooth 3D motion from a sequence of flat images. Doing this requires preloading all frames into memory, mapping scroll position to a frame index, and painting the correct frame to a `<canvas>` element fast enough to appear continuous.

This project implements that technique from scratch using the HTML5 Canvas 2D API, with no animation library handling the core render loop.

---

## How it works

**1. Frame preloading**

150 sequential WebP frames are loaded into `Image` objects at startup. URLs are generated programmatically from a zero-padded index template, so swapping the image sequence only requires changing the URL pattern and frame count.

```js
function files(index) {
  return `https://source.domain/assets/${String(index).padStart(4, '0')}.webp`
}

const images = []
for (let i = 1; i <= frameCount; i++) {
  const img = new Image()
  img.src = files(i)
  images.push(img)
}
```

**2. Scroll → frame index mapping**

Scroll position is normalized against the total scrollable height and multiplied by the frame count. The result is clamped to valid bounds and used as the index into the preloaded image array.

**3. Canvas paint**

`context.drawImage(images[frameIndex], 0, 0)` paints the correct frame on each scroll event. Canvas dimensions update on `window.resize` and re-render at the current frame index to prevent stretched output.

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| HTML5 Canvas 2D API | Frame rendering at scroll-driven speed |
| GSAP + ScrollTrigger | Supplementary scroll-linked UI animations |
| Vanilla JavaScript | Preload loop, scroll handler, resize observer |

---

## Getting Started

No build step. Open directly in a browser or serve with any static server.

```bash
git clone https://github.com/YOUR_USERNAME/canvas-scroll-sequence.git
cd canvas-scroll-sequence

npx serve .
# or open index.html directly
```

---

## Project Structure

```
canvas-scroll-sequence/
├── index.html      # Canvas element + CDN imports
├── style.css
└── script.js       # Frame preloader, scroll handler, canvas renderer
```

---

## Adapting to a different sequence

1. Replace the URL template in the `files(index)` function with your sequence's URL pattern
2. Update the `frameCount` constant to match your total frame count
3. Ensure frames are zero-padded to match the expected format (e.g., `0001.webp`)

For best performance, use WebP format and host on a CDN. Frame images should all be the same dimensions.

---

## Deploy

Drag the folder to [Netlify](https://netlify.com). No build command. Publish directory: `/`.

---

## License

MIT
