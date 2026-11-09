Save this as `index.html`. It’s self-contained and uses no external libraries or assets.

````html name=index.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="description" content="Calculate image dimensions for any aspect ratio. Enter a width or height, choose a popular preset, or set a custom ratio to get the matching pixel dimensions instantly.">
  <meta name="keywords" content="aspect ratio calculator, image dimension calculator, resize image, 16:9 calculator, TikTok dimensions, Instagram dimensions, video dimensions">
  <meta name="theme-color" content="#f5f7fb">
  <meta name="robots" content="index, follow">
  <title>Aspect Ratio &amp; Image Dimension Calculator</title>

  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "WebApplication",
    "name": "Aspect Ratio & Image Dimension Calculator",
    "description": "A free, browser-based calculator for finding image or video dimensions from a custom or preset aspect ratio.",
    "applicationCategory": "UtilitiesApplication",
    "operatingSystem": "Any",
    "browserRequirements": "Requires JavaScript",
    "offers": {
      "@type": "Offer",
      "price": "0",
      "priceCurrency": "USD"
    }
  }
  </script>

  <style>
    :root {
      color-scheme: light;
      --ink: #172033;
      --muted: #596579;
      --line: #dce2ec;
      --surface: #ffffff;
      --page: #f5f7fb;
      --accent: #315eea;
      --accent-dark: #2449bf;
      --accent-soft: #edf2ff;
      --success: #176b45;
      --error: #b42318;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      background: var(--page);
      color: var(--ink);
      line-height: 1.6;
    }

    .container {
      width: min(100% - 32px, 760px);
      margin-inline: auto;
    }

    header {
      padding: 38px 0 22px;
      text-align: center;
    }

    h1 {
      margin: 0 0 10px;
      font-size: clamp(1.8rem, 5vw, 2.6rem);
      line-height: 1.2;
      letter-spacing: -0.04em;
    }

    header p {
      max-width: 600px;
      margin: 0 auto;
      color: var(--muted);
    }

    main {
      padding-bottom: 48px;
    }

    .card,
    .content-section {
      background: var(--surface);
      border: 1px solid var(--line);
      border-radius: 18px;
      box-shadow: 0 8px 26px rgb(26 39 68 / 5%);
    }

    .card {
      padding: clamp(20px, 5vw, 32px);
    }

    h2 {
      margin: 0 0 8px;
      font-size: 1.35rem;
      line-height: 1.3;
    }

    .intro {
      margin: 0 0 24px;
      color: var(--muted);
    }

    .preset-label,
    .field-label {
      display: block;
      margin-bottom: 8px;
      font-size: 0.94rem;
      font-weight: 650;
    }

    .presets {
      display: flex;
      flex-wrap: wrap;
      gap: 9px;
      margin-bottom: 24px;
    }

    button {
      min-height: 44px;
      padding: 9px 14px;
      border: 1px solid var(--line);
      border-radius: 10px;
      background: #fff;
      color: var(--ink);
      font: inherit;
      font-weight: 600;
      cursor: pointer;
      touch-action: manipulation;
      transition: background-color 120ms ease, border-color 120ms ease, color 120ms ease;
    }

    button:hover {
      border-color: var(--accent);
      color: var(--accent-dark);
    }

    button:focus-visible,
    input:focus-visible {
      outline: 3px solid rgb(49 94 234 / 28%);
      outline-offset: 2px;
    }

    button[aria-pressed="true"] {
      border-color: var(--accent);
      background: var(--accent-soft);
      color: var(--accent-dark);
    }

    .ratio-row,
    .dimensions-row {
      display: grid;
      gap: 14px;
    }

    .ratio-row {
      grid-template-columns: minmax(0, 1fr) auto minmax(0, 1fr);
      align-items: end;
      max-width: 360px;
      margin-bottom: 22px;
    }

    .dimensions-row {
      grid-template-columns: repeat(2, minmax(0, 1fr));
    }

    .ratio-separator {
      padding-bottom: 9px;
      color: var(--muted);
      font-weight: 700;
    }

    input {
      width: 100%;
      min-height: 48px;
      padding: 10px 12px;
      border: 1px solid #c9d1df;
      border-radius: 10px;
      background: #fff;
      color: var(--ink);
      font: inherit;
    }

    input[aria-invalid="true"] {
      border-color: var(--error);
    }

    .unit {
      display: block;
      margin-top: 5px;
      color: var(--muted);
      font-size: 0.82rem;
    }

    .status {
      min-height: 25px;
      margin: 15px 0 0;
      color: var(--success);
      font-size: 0.92rem;
    }

    .status.error {
      color: var(--error);
    }

    .ad-placeholder {
      display: grid;
      min-height: 110px;
      place-items: center;
      margin: 22px 0;
      border: 1px dashed #b9c3d3;
      border-radius: 14px;
      background: #f9fafc;
      color: #788397;
      font-size: 0.88rem;
      text-align: center;
    }

    .content-section {
      margin-top: 22px;
      padding: clamp(20px, 5vw, 32px);
    }

    .content-section h2 {
      margin-top: 22px;
    }

    .content-section h2:first-child {
      margin-top: 0;
    }

    .content-section p {
      margin: 10px 0;
      color: #3f4a5d;
    }

    details {
      padding: 14px 0;
      border-top: 1px solid var(--line);
    }

    details:first-of-type {
      margin-top: 12px;
    }

    summary {
      font-weight: 650;
      cursor: pointer;
    }

    details p {
      margin-bottom: 0;
    }

    footer {
      padding: 0 0 26px;
      color: var(--muted);
      font-size: 0.86rem;
      text-align: center;
    }

    @media (max-width: 520px) {
      header {
        padding-top: 28px;
      }

      .dimensions-row {
        grid-template-columns: 1fr;
        gap: 16px;
      }

      .presets button {
        flex: 1 1 calc(50% - 9px);
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <h1>Aspect Ratio &amp; Image Dimension Calculator</h1>
      <p>Choose a familiar format or enter your own ratio. Set either dimension and get the proportional pixel size right away.</p>
    </header>

    <main>
      <section class="card" aria-labelledby="calculator-title">
        <h2 id="calculator-title">Calculate image dimensions</h2>
        <p class="intro">The ratio is written as width : height. Change either pixel value to calculate the other.</p>

        <span class="preset-label">Quick presets</span>
        <div class="presets" role="group" aria-label="Popular aspect ratio presets">
          <button type="button" data-width="16" data-height="9" aria-pressed="true">16:9 · YouTube</button>
          <button type="button" data-width="9" data-height="16" aria-pressed="false">9:16 · TikTok/Reels</button>
          <button type="button" data-width="4" data-height="3" aria-pressed="false">4:3 · Standard</button>
          <button type="button" data-width="1" data-height="1" aria-pressed="false">1:1 · Square</button>
          <button type="button" data-width="4" data-height="5" aria-pressed="false">4:5 · Instagram</button>
        </div>

        <div class="ratio-row">
          <div>
            <label class="field-label" for="ratioWidth">Ratio width</label>
            <input id="ratioWidth" type="number" min="0.01" step="any" value="16" inputmode="decimal">
          </div>
          <span class="ratio-separator" aria-hidden="true">:</span>
          <div>
            <label class="field-label" for="ratioHeight">Ratio height</label>
            <input id="ratioHeight" type="number" min="0.01" step="any" value="9" inputmode="decimal">
          </div>
        </div>

        <div class="dimensions-row">
          <div>
            <label class="field-label" for="imageWidth">Width</label>
            <input id="imageWidth" type="number" min="1" step="1" placeholder="e.g. 1920" inputmode="numeric">
            <span class="unit">pixels (px)</span>
          </div>
          <div>
            <label class="field-label" for="imageHeight">Height</label>
            <input id="imageHeight" type="number" min="1" step="1" placeholder="e.g. 1080" inputmode="numeric">
            <span class="unit">pixels (px)</span>
          </div>
        </div>

        <p id="status" class="status" role="status" aria-live="polite">Enter a width or height to get started.</p>
      </section>

      <aside class="ad-placeholder" aria-label="Advertisement placeholder">
        Ad space — reserved for a future Google AdSense placement
      </aside>

      <article class="content-section" aria-labelledby="guide-title">
        <h2 id="guide-title">A practical guide to aspect ratios and image dimensions</h2>
        <p>An aspect ratio describes the shape of an image or video: how wide it is compared with how tall it is. It is written as two numbers separated by a colon. In a 16:9 frame, for example, the width is sixteen parts and the height is nine parts. The ratio describes shape, not a particular resolution, so 1920 × 1080 and 1280 × 720 are both 16:9.</p>

        <p>This calculator helps when you know the frame shape and one pixel dimension, but not the other. Choose a preset or type your own ratio, then enter a width or height. The matching dimension updates automatically, rounded to the nearest whole pixel. If you edit the ratio after entering a size, the calculator uses the most recently edited dimension as its starting point.</p>

        <h2>Choosing a format for social media and design</h2>
        <p>Landscape video is often made in 16:9, a common choice for YouTube and conventional video players. A 1920 × 1080 canvas is a familiar full-HD example. Vertical content commonly uses 9:16; it fits the full-screen format used by TikTok, Instagram Reels, and YouTube Shorts. A 1080 × 1920 canvas is one widely used example, though platform recommendations and upload limits can change.</p>

        <p>Square images use 1:1, which works well for profile graphics, product tiles, and many feed layouts. The 4:5 portrait format is popular for Instagram feed posts because it takes up more vertical space on a phone than a square image. The classic 4:3 ratio appears in older displays, cameras, and some presentation layouts. These presets are convenient starting points, not a guarantee that every platform will display an upload identically.</p>

        <p>Apps such as Canva and CapCut let you choose a canvas or project ratio before you design or edit. Matching that canvas to the intended destination can reduce unwanted cropping and empty bars. If a design will be reused in several places, keep a copy of the original and make separate exports for each format. Check the current platform guidance before publishing campaign or client work, especially when safe areas, captions, or interface overlays matter.</p>

        <h2>The math behind the result</h2>
        <p>For a ratio written as <strong>ratio width : ratio height</strong>, the calculator uses straightforward proportions. When width is known, it calculates <strong>height = width × ratio height ÷ ratio width</strong>. When height is known, it calculates <strong>width = height × ratio width ÷ ratio height</strong>. For example, a 16:9 image that is 1600 pixels wide has a calculated height of 900 pixels. Results are rounded to the nearest whole pixel because image dimensions are generally entered as whole pixels.</p>

        <p>This tool calculates dimensions only; it does not upload, resize, or alter your image. The result gives you a target canvas size to enter in your preferred editor. Keep in mind that cropping an existing image to a new ratio may remove content near its edges, while stretching it can make people and objects look distorted. To preserve the composition, adjust the canvas or reposition the image rather than forcing it to fill a mismatched frame.</p>

        <h2>Frequently asked questions</h2>
        <details>
          <summary>Will changing the width change the height automatically?</summary>
          <p>Yes. Once you enter a valid width, the height is calculated from the selected ratio. Entering a height instead calculates the width.</p>
        </details>
        <details>
          <summary>Can I use a custom aspect ratio?</summary>
          <p>Yes. Enter any positive numbers in the ratio width and ratio height fields, such as 3:2 or 2.35:1. The calculator treats them as a width-to-height proportion.</p>
        </details>
        <details>
          <summary>Does this tool resize my image?</summary>
          <p>No. It runs in your browser and calculates pixel dimensions; it does not select, upload, or edit an image file.</p>
        </details>
        <details>
          <summary>Why might a platform crop my correctly sized image?</summary>
          <p>Platforms may display different crops in previews, feeds, or on different devices. Keep important text and subjects away from the outer edges, and check the platform’s latest publishing specifications.</p>
        </details>
      </article>
    </main>

    <footer>
      A free, browser-based calculator for image and video dimensions.
    </footer>
  </div>

  <script>
    (() => {
      const ratioWidth = document.getElementById("ratioWidth");
      const ratioHeight = document.getElementById("ratioHeight");
      const widthInput = document.getElementById("imageWidth");
      const heightInput = document.getElementById("imageHeight");
      const status = document.getElementById("status");
      const presetButtons = document.querySelectorAll(".presets button");

      let lastEditedDimension = "width";

      function setStatus(message, isError = false) {
        status.textContent = message;
        status.classList.toggle("error", isError);
      }

      function validRatio() {
        const rw = Number(ratioWidth.value);
        const rh = Number(ratioHeight.value);
        const valid = Number.isFinite(rw) && Number.isFinite(rh) && rw > 0 && rh > 0;

        ratioWidth.setAttribute("aria-invalid", String(!valid));
        ratioHeight.setAttribute("aria-invalid", String(!valid));
        if (!valid) {
          setStatus("Enter positive numbers for both parts of the aspect ratio.", true);
        }
        return valid ? { rw, rh } : null;
      }

      function calculate(source) {
        const ratio = validRatio();
        if (!ratio) return;

        const { rw, rh } = ratio;
        const sourceInput = source === "width" ? widthInput : heightInput;
        const targetInput = source === "width" ? heightInput : widthInput;

        if (sourceInput.value === "") {
          targetInput.value = "";
          setStatus("Enter a width or height to get started.");
          return;
        }

        const sourceValue = Number(sourceInput.value);
        if (!Number.isFinite(sourceValue) || sourceValue <= 0) {
          targetInput.value = "";
          setStatus("Enter a pixel dimension greater than zero.", true);
          return;
        }

        const result = source === "width"
          ? Math.round(sourceValue * rh / rw)
          : Math.round(sourceValue * rw / rh);

        targetInput.value = String(Math.max(1, result));
        setStatus(`Calculated ${widthInput.value} × ${heightInput.value} px at ${rw}:${rh}.`);
      }

      widthInput.addEventListener("input", () => {
        lastEditedDimension = "width";
        calculate("width");
      });

      heightInput.addEventListener("input", () => {
        lastEditedDimension = "height";
        calculate("height");
      });

      function onRatioChange() {
        presetButtons.forEach(button => button.setAttribute("aria-pressed", "false"));

        const hasWidth = widthInput.value !== "";
        const hasHeight = heightInput.value !== "";
        if (!hasWidth && !hasHeight) {
          validRatio();
          if (Number(ratioWidth.value) > 0 && Number(ratioHeight.value) > 0) {
            setStatus("Enter a width or height to get started.");
          }
          return;
        }

        const source = lastEditedDimension === "height" && hasHeight ? "height" : "width";
        if (source === "width" && !hasWidth && hasHeight) {
          calculate("height");
        } else {
          calculate(source);
        }
      }

      ratioWidth.addEventListener("input", onRatioChange);
      ratioHeight.addEventListener("input", onRatioChange);

      presetButtons.forEach(button => {
        button.addEventListener("click", () => {
          ratioWidth.value = button.dataset.width;
          ratioHeight.value = button.dataset.height;
          presetButtons.forEach(item => {
            item.setAttribute("aria-pressed", String(item === button));
          });

          if (widthInput.value !== "" || heightInput.value !== "") {
            const source = lastEditedDimension === "height" && heightInput.value !== ""
              ? "height"
              : "width";
            if (source === "width" && widthInput.value === "" && heightInput.value !== "") {
              calculate("height");
            } else {
              calculate(source);
            }
          } else {
            setStatus(`Aspect ratio set to ${button.dataset.width}:${button.dataset.height}. Enter a width or height.`);
          }
        });
      });
    })();
  </script>
</body>
</html>
````
