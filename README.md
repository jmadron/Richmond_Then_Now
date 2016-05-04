#Urban Scratch-off

Test it here [http://chriswhong.github.io/urbanscratchoff/](http://chriswhong.github.io/urbanscratchoff/)

##About

Richmond was a very different place a decade after the Civil War than it is today.  When we started working on the Visualizing the Past project for the Library of Virginia, we found a wonderful atlas of Richmond in 1876. This hand painted atlas, published by F.W Beers, features detailed buildings and their owners, parks and public landmarks. These maps served as our basemap for the project because of the great detail they provided. While working on the project I became really interested in what has changed around Richmond since this time.

###Methodology
Full Disclosure: This code was adapted from Chris Whong. The map is loading two layers.  The bottom layer is a normal leaflet.js TileLayer.  The top layer makes use of leaflet's Canvas TileLayer, essentially setting up a grid of 256x256 HTML canvases.  For each canvas, a tile image is requested from the top layer, and is drawn into the canvas.  When the user "Scratches" the canvas, they are drawing into the canvas using the 'destination-out' composite operation.  This turns the affected area transparent, allowing the bottom layer to show through! Tiles were generated for the historical map using gdal2tiles.py.
[Leaflet.CanvasLayer](https://github.com/CartoDB/Leaflet.CanvasLayer) plugin adds a canvas overlay to a leaflet map, and gives you the bounds of the current view to use when transforming whatever you are going to render.

Code below is basically an adaptation of leaflet's native `imageOverlay` method, providing a size and offset based on the current map view.  These are passed into canvas `drawImage()` to draw the overlay from the png.

```
function drawingOnCanvas(canvasOverlay, params) {
          console.log('drawingOnCanvas')
          console.log(params);
          var ctx = params.canvas.getContext('2d');

          ctx.fillCircle = fillCircle;
      

          ctx.clearRect(0, 0, params.canvas.width, params.canvas.height); 

          var bounds = new L.Bounds(
            map.latLngToContainerPoint(imgBounds.getNorthWest()),
            map.latLngToContainerPoint(imgBounds.getSouthEast()));
          
          var size = bounds.getSize();
          console.log(size);

          ctx.drawImage(img,bounds.min.x,bounds.min.y,size.x,size.y);
}
```

"erasing" parts of the canvas overlay is done by setting a circle under the mouse to trasnparent, [this process.](http://jsfiddle.net/ArtBIT/WUXDb/1/)


