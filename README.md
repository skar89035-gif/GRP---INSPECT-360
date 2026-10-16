# Basic Instrument Dimensional Inspection Simulation

A browser simulation of an X/Y digital-caliper and Z-indicator rig. It imports a STEP/STP model, calculates its axis-aligned X/Y/Z extents in millimetres, animates the rig through those dimensions, and compares the results with drawing nominals using a ±0.05 mm tolerance.

## Run locally

Serve the folder on localhost (for example, run `python -m http.server 8000`) and open `http://localhost:8000`. The model preview uses Three.js and the STEP importer from jsDelivr, so internet access is needed to render STEP/STP files. The measuring rig controls work independently of the model preview.

## Demonstration workflow

1. Upload a STEP/STP model. Its geometry extents appear in the part preview and enable the rig.
2. Enter the drawing nominals for length X, width Y, and height Z.
3. Select **MEASURE X / Y / Z**. The rig animates X and Y caliper travel and Z indicator contact. The results table reports measured value, nominal, deviation, and PASS/FAIL for each axis.
4. Review detected vertical cylindrical holes for fitted diameter and straight-wall depth.
5. Export the captured values as CSV.

The dimensions are calculated from the uploaded model geometry; the rig motion is simulated and no physical instruments are connected. Model extents are not a physical inspection of a manufactured part. The tolerance check is inclusive at nominal ±0.05 mm.

## Model upload

Choose or drag a STEP/STP file into the part upload area to render and orbit the uploaded geometry. DWG files can be selected and are identified in the preview panel, but the browser does not decode native DWG geometry; export the drawing from CAD software to STEP/STP to view the 3D part.

## Hole measurement

The app looks for vertical cylindrical internal faces in STEP geometry, fits their XY circle to estimate bore diameter, and uses the cylindrical face height as straight-wall depth. Results include the feature center in model coordinates. Angled holes, non-cylindrical holes, and features without enough tessellated side-wall geometry may not be detected. Dimensions come from CAD geometry; the simulated stylus does not physically probe the hole.
