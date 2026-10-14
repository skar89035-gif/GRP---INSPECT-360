# GRP Single-Model Dimensional Inspection

This browser app loads one measured geometry file, displays the part, measures its overall extents, and checks drawing dimensions against a fixed ±0.05 mm acceptance band. It also has a two-click 3D point-to-point tool for a selected feature, such as the distance across a component or between two points on a slot.

The app does not make up models or measurements. It starts with blank drawing-value fields and no PASS/FAIL result. Measurements appear only after a real file is loaded. PASS/FAIL appears only for dimensions with a nominal value entered by the operator.

## Run locally

The browser app uses ES modules and WebAssembly, so serve this folder over localhost instead of opening `index.html` directly.

1. Open PowerShell in this folder.
2. Run `python -m http.server 8000`.
3. Open `http://localhost:8000` in Chrome, Edge, or Firefox.
4. Keep internet access available for the Three.js viewer and CAD importer dependencies loaded from jsDelivr.

## Operator workflow

1. Load one **as-built measured model**. The file should be geometry captured from the actual part, such as a scan or metrology output. A nominal design CAD file only measures that design file; it does not prove what was manufactured.
2. Select the units used by the mesh. STL and OBJ normally do not preserve reliable unit metadata. The STEP/IGES importer is asked to return coordinates in the selected unit. All displayed and checked dimensions are converted to millimetres.
3. The app computes the model's axis-aligned X, Y, and Z extents in the uploaded file's coordinate system.
4. Enter drawing nominal values for Length (X), Width (Y), and/or Height (Z). Blank fields are reported as measured only and do not get a pass/fail label.
5. For a local feature dimension, enter a feature name and its nominal size, choose **POINT MEASURE**, then click two locations on the visible surface. The app computes the Euclidean distance between those two picked surface points.
6. Each entered target is checked against `nominal - 0.05 mm` through `nominal + 0.05 mm`, inclusive. Export the computed table to CSV for the part record.

For the example nominal length of 20 mm, the accepted band is 19.9500 mm to 20.0500 mm. The target itself is never prefilled as a measured result. The operator must enter the nominal from the drawing, then the model supplies the measured value.

## Supported geometry

| File type | Behavior |
| --- | --- |
| STL | Binary or ASCII triangle mesh import |
| OBJ | Polygon mesh import |
| STEP / STP | Browser CAD import through OpenCascade WebAssembly, then display and bounding-box measurement |
| IGES / IGS | Browser CAD import through OpenCascade WebAssembly, then display and bounding-box measurement |
| DWG / IPT | Not decoded in the browser. Export to STEP or STL in CAD software first. |

Each input is limited to 100 MB and 500,000 triangles to keep browser memory use bounded. STEP/IGES is tessellated for display. That tessellation and the source file's accuracy affect the values.

## How the measurements work

**Overall size:** the app computes a world-coordinate bounding box of the imported geometry and reports its X, Y, and Z spans in millimetres. These are axis-aligned extents, not feature recognition. If the model is rotated relative to its drawing axes, the extents may not correspond to drawing length/width/height; orient the model consistently before inspection.

**Selected feature:** point picking raycasts against the displayed triangulated surface. The measured value is the straight-line distance between the two hit locations. It is useful for simple distances, but it is not automatic hole/slot detection, circle fitting, true-position analysis, or a datum-based GD&T evaluation.

The CSV includes timestamp, model filename and format, unit interpretation, fixed tolerance, checked dimensions, measured and nominal values, deviation, acceptance interval, and per-dimension result. No CSV can be exported until a model has been measured.

## Engineering limits

The challenge brief describes checking real GRP parts against a drawing and producing a traceable per-part GD&T record; it calls out hole and slot position/size, perpendicularity, and surface profile as important requirements. This app is a software geometry-measurement interface only. It does not include a scanner, camera, physical sensor, part fixturing, drawing/PDF dimension extraction, feature recognition, datum alignment, full GD&T calculations, uncertainty budget, calibration, traceability, or certification.

A result within ±0.05 mm only says the imported geometry's computed dimension is within that numeric band. It does **not** establish that the physical part is accurate to 50 microns. That depends on the measurement equipment, calibration, scanning procedure, GRP surface, data registration, and source mesh fidelity. Do not label this app certified or production-ready metrology without validating the full measurement chain.

## Why there is no ML/ChatGPT result engine

ML is not needed for measuring a known distance or comparing a computed dimension with a numeric tolerance. The numerical result should be reproducible geometry arithmetic. AI can help an operator find fields or explain a report, but must not invent measurements, infer missing nominal dimensions, or choose PASS/FAIL. Drawing dimensions are entered by an operator in this version; they are not extracted from the PDF.

## Project files

- `index.html` — single-model workflow and interface.
- `app.js` — supported CAD/mesh import, 3D display, actual geometry measurements, tolerance checks, and CSV output.
- `styles.css` and `dimensional.css` — responsive interface styles.

## Runtime libraries

- [Three.js](https://threejs.org/) for the 3D viewer and mesh file loaders.
- [occt-import-js](https://github.com/kovacsv/occt-import-js) for STEP/IGES import in the browser.

Dependencies load at runtime from jsDelivr. For offline or controlled deployments, review the licenses and self-host pinned library assets.
