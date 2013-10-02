clmtrackr
======

![tracked face](http://auduno.github.com/clmtrackr/media/clmtrackr_03.jpg)

**clmtrackr** is a javascript library for fitting facial models to faces in videos or images. It currently is an implementation of *constrained local models* fitted by *regularized landmark mean-shift*, as described in [Jason M. Saragih's paper](http://dl.acm.org/citation.cfm?id=1938021). **clmtrackr** tracks a face and outputs the coordinate positions of the face model as an array, following the numbering of the model below:

[![facemodel_numbering](http://auduno.github.com/clmtrackr/media/facemodel_numbering_new_small.png)](http://auduno.github.com/clmtrackr/media/facemodel_numbering_new.png)

The library provides some generic face models that were trained on [the MUCT database](http://www.milbo.org/muct/) and some additional self-annotated images. The aim is to also provide a model builder for building your own facial models.

The library requires [ccv.js](https://github.com/liuliu/ccv) (for initial face detection) and [numeric.js](http://numericjs.com) (for matrix math).

For tracking in video, it is recommended to use a browser with WebGL support, though the library should work on any modern browser.

[Reference](http://auduno.github.io/clmtrackr/docs/reference.html)

For some more information about Constrained Local Models, take a look at Xiaoguang Yan's [excellent tutorial](https://sites.google.com/site/xgyanhome/home/projects/clm-implementation/ConstrainedLocalModel-tutorial%2Cv0.7.pdf?attredirects=0), which was of great help in implementing this library.

### Examples ###

* [Tracking in image](http://auduno.github.com/clmtrackr/clm_image.html)
* [Tracking in video](http://auduno.github.com/clmtrackr/clm_video.html)
* [Face masking](http://auduno.github.com/clmtrackr/face_mask.html)
* [Face deformation](http://auduno.github.com/clmtrackr/face_deformation_video.html)

### Usage ###

Download the minified library [clmtrackr.js](https://github.com/auduno/clmtrackr/raw/dev/clmtrackr.js) and one of the models, and include them in your webpage. **clmtrackr** depends on [*numeric.js*](https://github.com/sloisel/numeric/) and [*ccv.js*](https://github.com/liuliu/ccv), but these are included in the minified library.

```html
/* clmtrackr libraries */
<script src="js/clmtrackr.js"></script>
<script src="js/model_pca_20_svm.js"></script>
```

The following code initiates the clmtrackr with the model we included, and starts the tracker running on a video element.

```html
<video id="inputVideo" width="400" height="300" autoplay loop>
  <source src="./media/somevideo.ogv" type="video/ogg"/>
</video>
<script type="text/javascript">
  var videoInput = document.getElementById('video');
  
  var ctracker = new clm.tracker();
  ctracker.init(pModel);
  ctracker.start(videoInput);
</script>
```

You can now get the positions of the tracked facial features as an array via ```getCurrentPosition()```:

```html
<script type="text/javascript">
  function positionLoop() {
    requestAnimationFrame(positionLoop);
    var positions = ctracker.getCurrentPosition();
    // positions = [[x_0, y_0], [x_1,y_1], ... ]
    // do something with the positions ...
  }
  positionLoop();
</script>
```

You can also use the built in function ```draw()``` to draw the tracked facial model on a canvas :

```html
<canvas id="drawCanvas" width="400" height="300"></canvas>
<script type="text/javascript">
  var canvasInput = document.getElementById('canvas');
  var cc = canvasInput.getContext('2d');
  function drawLoop() {
    requestAnimationFrame(drawLoop);
    cc.clearRect(0, 0, canvasInput.width, canvasInput.height);
    ctracker.draw(canvasInput);
  }
  drawLoop();
</script>
```

See the complete example [here](http://auduno.github.com/clmtrackr/example.html).

### License ###

**clmtrackr** is distributed under the [MIT License](http://www.opensource.org/licenses/MIT)