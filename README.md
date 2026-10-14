# GRP Single-Model Dimensional Inspection

### 🚀 Live Demo

**[Open GRP INSPECT 360](https://grpinspector.vercel.app/)**

A browser-based dimensional inspection application for measured GRP (Glass Reinforced Plastic) geometry. The application loads a single measured geometry file, displays the component in 3D, calculates overall dimensions, and checks entered drawing dimensions against a fixed ±0.05 mm acceptance band.

> **Important:** The application does not generate or invent models or measurements. Measurements are calculated only from the geometry file uploaded by the operator.

---

## 📌 Project Overview

GRP INSPECT 360 is a web-based dimensional inspection tool designed to support the inspection of large GRP components.

The application allows an operator to:

- Load a measured geometry file
- Visualize the component in 3D
- Calculate overall X, Y, and Z dimensions
- Enter nominal drawing dimensions
- Compare measured values against a ±0.05 mm tolerance
- Measure the distance between two selected points on the model
- Generate a CSV inspection record

The system is intended as a **software-based geometry inspection interface** and does not replace calibrated metrology equipment.

---

## ⚙️ Key Features

- 3D geometry visualization
- Single-model inspection workflow
- Overall X, Y and Z dimensional measurement
- Two-point 3D distance measurement
- ±0.05 mm acceptance band
- Operator-entered nominal drawing values
- PASS/FAIL evaluation
- CSV inspection report generation
- Unit selection and conversion to millimetres
- Support for mesh and CAD geometry files
- No fabricated measurements or automatic PASS/FAIL without nominal values

---

## 📂 Supported Geometry

| File Type | Support |
|---|---|
| STL | Binary and ASCII triangle mesh |
| OBJ | Polygon mesh |
| STEP / STP | Browser CAD import |
| IGES / IGS | Browser CAD import |
| DWG / IPT | Not directly supported; export to STEP or STL first |

### File Limits

- Maximum file size: **100 MB**
- Maximum triangles: **500,000**

STEP and IGES models are tessellated for browser display, so the tessellation and source-file accuracy can affect measurements.

---

## 🔍 Operator Workflow

### 1. Load the measured model

Upload an **as-built measured model** obtained from the actual manufactured component, such as scan or metrology output.

A nominal CAD design file only measures the design geometry; it does not prove the dimensions of the manufactured part.

### 2. Select units

Select the units corresponding to the uploaded geometry.

STL and OBJ files generally do not contain reliable unit metadata, so the operator must select the appropriate interpretation.

### 3. Measure overall dimensions

The application calculates the model's axis-aligned:

- X extent
- Y extent
- Z extent

All displayed values are converted to millimetres.

### 4. Enter drawing dimensions

The operator enters the nominal Length, Width and/or Height values from the engineering drawing.

Blank fields are displayed as measured values only and are not assigned PASS/FAIL.

### 5. Measure a selected feature

For a local feature:

1. Enter the feature name.
2. Enter the nominal dimension.
3. Select **POINT MEASURE**.
4. Click two locations on the visible surface.
5. The application calculates the Euclidean distance between the two selected points.

### 6. Generate inspection record

The measured values and inspection results can be exported as a CSV file.

---

## 📏 Tolerance Evaluation

The application uses a fixed acceptance band of:

**Nominal ± 0.05 mm**

For example, for a nominal dimension of **20.00 mm**:

```text
Lower limit = 19.9500 mm
Upper limit = 20.0500 mm
```

The measured value is evaluated against this range.

The nominal value is **never automatically used as the measured value**. The operator must enter the drawing nominal, while the application calculates the measured value from the uploaded geometry.

---

## 📐 Measurement Method

### Overall Dimensions

The application calculates the world-coordinate bounding box of the imported geometry and reports the X, Y and Z spans.

These are **axis-aligned extents**, not automatic feature recognition.

If the model is rotated relative to the engineering drawing axes, the reported dimensions may not correspond to drawing Length, Width and Height.

### Selected Feature Measurement

The point measurement tool uses raycasting against the displayed triangulated surface.

The measured value is the straight-line Euclidean distance between the two selected surface points.

It is suitable for simple point-to-point measurements but does not perform:

- Automatic hole detection
- Slot recognition
- Circle fitting
- Datum alignment
- True-position calculation
- Full GD&T evaluation

---

## 📊 CSV Inspection Report

The generated CSV record contains information such as:

- Timestamp
- Model filename
- File format
- Unit interpretation
- Tolerance
- Nominal dimension
- Measured dimension
- Deviation
- Acceptance interval
- PASS/FAIL result

A CSV report cannot be generated until a model has been measured.

---

## 🛠️ Technologies Used

- HTML
- CSS
- JavaScript
- Three.js
- WebAssembly
- OpenCascade
- occt-import-js
- Vercel

### Runtime Libraries

**Three.js**  
Used for 3D visualization and mesh file loading.

**occt-import-js**  
Used for STEP/IGES CAD import through OpenCascade WebAssembly.

Dependencies are loaded at runtime through jsDelivr.

---

## 🌐 Deployment

The application is deployed using Vercel.

### Live Application

**[https://grpinspector.vercel.app/](https://grpinspector.vercel.app/)**

### Source Code

**[GitHub Repository](https://github.com/skar89035-gif/GRP---INSPECT-360)**

---

## 💻 Run Locally

The application uses ES modules and WebAssembly, so it should be served through a local HTTP server instead of opening `index.html` directly.

### Step 1 — Open PowerShell

Navigate to the project folder.

### Step 2 — Start the local server

```bash
python -m http.server 8000
```

### Step 3 — Open the application

Open:

```text
http://localhost:8000
```

Use Chrome, Edge or Firefox.

Internet access is required for the Three.js viewer and CAD importer dependencies loaded from jsDelivr.

---

## 📁 Project Structure

```text
GRP---INSPECT-360/
│
├── index.html
├── app.js
├── styles.css
├── dimensional.css
└── README.md
```

---

## ⚠️ Engineering Limitations

The application is a **software geometry-measurement interface**.

It does not currently include:

- Physical scanner or camera
- Physical measurement sensors
- Part fixturing
- Drawing/PDF dimension extraction
- Automatic feature recognition
- Datum alignment
- Complete GD&T calculations
- Uncertainty budget
- Calibration management
- Measurement traceability
- Metrology certification

A result within **±0.05 mm** only means that the computed geometry dimension falls within the specified numerical band.

It does **not** establish that the physical GRP component is accurate to 50 microns.

Physical accuracy depends on factors including:

- Measurement equipment
- Equipment calibration
- Scanning procedure
- GRP surface characteristics
- Data registration
- Source mesh accuracy
- Measurement uncertainty

Therefore, this application should not be described as certified or production-ready metrology without validation of the complete measurement chain.

---

## 🤖 Why There Is No AI/ML Measurement Engine?

AI/ML is not required for calculating a known geometric distance or comparing that measurement against a numerical tolerance.

The measurement result should be reproducible mathematical geometry.

AI can potentially assist an operator with:

- Finding fields
- Explaining inspection reports
- Guiding the inspection workflow

However, AI should not:

- Invent measurements
- Infer missing nominal dimensions
- Generate models
- Automatically choose PASS/FAIL without valid measurement data

In this version, drawing dimensions are entered by the operator and are not automatically extracted from PDFs.

---

## 🎯 Project Objective

The objective of GRP INSPECT 360 is to provide a browser-based, traceable and transparent interface for dimensional inspection of measured GRP geometry while clearly separating:

**Measured geometry → Nominal drawing value → Tolerance evaluation → Inspection result**

---

## 👥 Project

**GRP INSPECT 360**

Developed as a digital inspection and measurement solution for large GRP component dimensional assessment.
