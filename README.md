AI built a browser-based SVG layer painter for pen plotter prep. The whole point is to take one SVG and quickly sort the artwork into separate color layers before opening it in Inkscape.

The app lets me drag and drop an SVG file into the page. Once it loads, the SVG shows up in the workspace, and I paint over the parts I want assigned to a specific pen color. The painting does not become the final artwork. It only acts as a selection system. When I paint over part of the SVG, the app uses the paint color to decide which Inkscape layer each SVG shape belongs to.

The brush stays inside the actual SVG artwork. I cannot paint across empty white space and accidentally mark blank areas. The app builds a hidden silhouette mask from the SVG geometry, then clips the paint against that mask. So even if I drag the brush outside the lines, the paint only sticks where real SVG shapes exist.

I added a Photoshop-style brush cursor too. Instead of only seeing a crosshair, I see a circle showing the real brush size. When I move the brush size slider, the circle gets bigger or smaller. When I switch colors, the cursor ring matches the active layer color. When I switch to eraser, the cursor turns red. It gives me instant feedback without looking back at the toolbar.

The layer system is flexible, but capped at five layers so it stays practical for pen plotting. I can add layers, pick a color for each layer, and the exported Inkscape layer name becomes the color I picked, like #8b5cf6 or #f97316. That makes the output easy to understand when I open it later in Inkscape.

The toolbar has physical pen width presets too. Instead of guessing random pixel sizes, I can pick common pen sizes like 0.3mm, 0.4mm, 0.5mm, 0.8mm, or 1.0mm. There is still a custom slider if I want manual control.

I also added keyboard shortcuts so I do not have to keep moving away from the canvas. B switches back to brush, E switches to eraser, [ and ] shrink or grow the brush, and C clears the paint.

When I click Generate, the app checks each SVG shape and samples the painted pixels over it. It uses the real SVG geometry, not just the bounding box, so it avoids assigning weird shapes wrong. For example, it handles diagonal lines, open paths, curves, and shapes where the center of the box might not actually be part of the artwork. That fix came from the review notes about using isPointInFill() and isPointInStroke() before sampling.

Before export, the app runs a pre-flight check. If there are unassigned shapes, it warns me instead of silently exporting a broken file. I can cancel, export anyway, or push every unassigned shape into the active layer. That saves me from opening the file in Inkscape and realizing I missed a bunch of tiny paths.

The exported SVG is built for Inkscape. Each color group becomes a real Inkscape layer using inkscape:groupmode="layer" and inkscape:label. It also includes the Inkscape and Sodipodi namespaces so Inkscape reads the layers correctly.

The export does not save my paint strokes as blobs or raster data. It keeps the original SVG geometry. The paint only decides which layer each shape goes into. Then the app converts those shapes into clean colored line art by setting the stroke color to the layer color and removing the fill. That matters because the final file stays vector-based and plotter-ready.

I also added a loading overlay for bigger files. When I drop in a heavy SVG, it shows messages like “Parsing Geometry” and “Building Paint Mask” instead of looking frozen. The resize logic also preserves the paint, so if the browser window changes size, the app rebuilds the SVG mask first, then restores the paint correctly instead of wiping or clipping it wrong.
