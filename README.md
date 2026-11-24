# YOLO26 Studio — GitHub Pages Starter

A responsive, client-side YOLO26 inference template built from the supplied AegisMask prototype and generalized into a reusable development starter.

It runs directly in the browser. There is no Python server, API endpoint, or backend inference service. The UI uses the official `@ultralytics/yolo` browser inference package and an ONNX model hosted by the same GitHub Pages site.

## Features

- YOLO26 ONNX inference in the browser.
- WebGPU when available, with automatic CPU/WASM fallback.
- Webcam inference with front/back camera selection.
- Single-image inference from a local file.
- Load the default repository model or select a different local `.onnx` file.
- Adjustable confidence threshold.
- Bounding-box annotation, class labels, confidence scores, detection list, inference latency, and approximate live FPS.
- Responsive layout for desktop, tablet, and mobile.
- GitHub Actions workflow for automatic GitHub Pages deployment.

## 1. Add the default model

The default configuration expects this exact path:

```text
models/yolo26n.onnx
```

**Download:** [YOLO26n ONNX — direct file (9.94 MB)](https://huggingface.co/prithivMLmods/YOLO26-ONNX/resolve/main/yolo26n/yolo26n.onnx)

Save the downloaded file as `yolo26n.onnx` inside this repository's `models/` directory. The linked Hugging Face repository publishes this file and its SHA-256 checksum. It is a community-hosted copy, not a download served from the official Ultralytics domain:

```text
00e2d1062178fca312fee35cf5fc9b9c783b3e5c9675d48b84929f69ee391807
```

Verify the checksum after downloading if you use that mirror. Do not assume a mirror's metadata overrides the upstream model license: Ultralytics states that its trained YOLO models are AGPL-3.0 by default, with an Enterprise option for use cases that need different terms. Review the upstream terms before redistribution or commercial deployment.

> **Naming note:** The standard Ultralytics nano detection model is named `yolo26n.onnx`, not `yolo26.onnx`. The `n` denotes the nano model size. This is the default general-purpose COCO object-detection model used by this starter. YOLO26 also has `s`, `m`, `l`, and `x` sizes and task-specific variants.

### Export it yourself (recommended for reproducibility)

For a model generated directly from the official Ultralytics checkpoint, export it yourself with a recent Ultralytics version:

```bash
python -m pip install --upgrade ultralytics
```

```python
from ultralytics import YOLO

model = YOLO("yolo26n.pt")
model.export(format="onnx")  # creates yolo26n.onnx
```

Move the resulting `yolo26n.onnx` into `models/`. The current Ultralytics ONNX documentation specifies that the export embeds model metadata such as task, class names, and image size. The browser inference package expects a compatible Ultralytics ONNX export; use Ultralytics `>=8.4.142` when exporting for the browser package.

Official references:

- [YOLO26 model documentation](https://docs.ultralytics.com/models/yolo26/)
- [YOLO26 ONNX export guide](https://docs.ultralytics.com/integrations/onnx)
- [Ultralytics browser inference package](https://github.com/ultralytics/inference/tree/main/web)

## 2. Configure the model path

The default path is configured near the top of `app.js`:

```js
const CONFIG = Object.freeze({
  defaultModelPath: "models/yolo26n.onnx",
  defaultConfidence: 0.25,
  // ...
});
```

Change `defaultModelPath` if you use a different file name or folder. You can also use **Use a different model** in the UI to load an ONNX file from your device for the current browser session.

The path is resolved relative to `app.js`, so it continues to work when GitHub Pages serves the project under `https://<username>.github.io/<repository>/`.

## 3. Run locally

Camera access requires a secure context. Use HTTPS or `localhost`; opening `index.html` directly as a `file://` URL may prevent camera access.

For example, with Python:

```bash
python -m http.server 8000
```

Then open [http://localhost:8000](http://localhost:8000).

The first model initialization also downloads the browser inference runtime/WASM assets from the configured CDN. The browser can cache those assets for later sessions. An internet connection is required for the CDN runtime unless you configure and host the runtime assets yourself.

## 4. Deploy to GitHub Pages

1. Create a new GitHub repository and push the contents of this folder to its `main` branch.
2. Ensure `models/yolo26n.onnx` is committed (or use the UI's local model picker for a session-specific test).
3. Open **Settings → Pages** in the repository.
4. Under **Build and deployment**, set **Source** to **GitHub Actions**.
5. Push to `main`. The included workflow at `.github/workflows/pages.yml` builds the static site artifact and deploys it.
6. When the workflow completes, open the Pages URL shown in the repository's Pages settings or in the workflow deployment summary.

The app is a static site: do not select a Python, Node, or other server build unless you intentionally add a build system later.

## 5. Project structure

```text
.
├── .github/
│   └── workflows/
│       └── pages.yml
├── models/
│   └── yolo26n.onnx       # add this model file
├── app.js                 # YOLO26 loading and inference logic
├── index.html             # application structure
├── style.css              # responsive visual system
└── README.md
```

## 6. Browser compatibility and notes

- Use a modern browser with JavaScript modules enabled.
- WebGPU availability depends on the browser, device, drivers, and secure context. When WebGPU is unavailable, the inference library can fall back to CPU/WASM.
- Camera access requires user permission and HTTPS or `localhost`.
- Performance varies by device, browser, model size, and input resolution. Start with YOLO26n for a lightweight baseline.
- GitHub release asset URLs cannot be fetched directly by browser code when their response lacks CORS headers. Hosting the ONNX file in this repository keeps it same-origin with the GitHub Pages app.
- The template does not upload camera frames or selected images to an application backend. The browser still downloads the model/runtime files from the configured hosting/CDN sources.

## 7. Customize it

- **Model:** update `CONFIG.defaultModelPath` in `app.js`.
- **Confidence:** change `CONFIG.defaultConfidence`.
- **Annotation:** adjust `CONFIG.annotation`.
- **Layout and theme:** edit `style.css`.
- **Application identity:** update the title, header, metadata, and footer in `index.html`.
- **Task-specific outputs:** the UI currently presents detection boxes and classification top predictions. Extend `updateDetections()` for additional task-specific UI such as segmentation masks, pose keypoints, OBB, semantic segmentation, or depth output.
