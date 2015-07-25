## Snaptoolkit

## A toolkit of stuff relating to Snap.svg

This is less of a release, and more of a toolkit of possible solutions to common problems that appear in Snap that people could use and abuse and incorporate. I'm not sure I'll have time to ever do a proper release (unless others wanted to help), so its more of a chunk of code to get improved on over time to come up with the most correct way of doing something (as some transform integration is difficult to understand).

Ideas include...
- Snap/pan/zoom example on elements and pan/zoom on svg elements (as you can't transform an svg)
- Basic freetransform implementation
- Allowing snap/pan/zoom to work with Hammer.js to deal with touch interfaces
- Make animated sequences easier (there is no timeline type thing as yet)
- Allow easier SMIL markup animation
- Rotate/Pan/Zoom immediate and animated
- Add a set of plugins to add extra features to to be written quicker


- Example at http://svg.dabbles.info/snaptoolkit/snappan.html

## Methods

- el.animateFrames( arrayOfAnimations ) // sequences of animations in order, using callbacks after each
-    frame consists of
-    el: element to animate
-    animation: the animation, eg { transform: 't20,20' }
-    dur: duration
-    easing: easing (snap easings)
-    startFunc: function to call on the start of the next frame
-    You could probably put a null animation in to fake a pause and endFunc
```
c.animateFrames([
    { el: c,      animation: { transform: 's0.4,0.4,400,100' },         dur: 1000, easing: mina.bounce },
    { el: c,      animation: { transform: 't50,500' },                dur: 1000, easing: mina.bounce },
    { el: myRect, animation: { transform: 's1.2,1.2,300,300t200,-100' },dur: 1000, easing: mina.bounce, startFunc: myFunc }
]);
```

- el.getEventPoint( event ) // get the event point in elements coordinate space from screen (may need to rephrase)
- el.getMousePoiunt( ev, x, y ) // get the event accounting for screen/paper/element transform
- el.getInverseScreenPoint( x, y ) // get the point accounting for screen inverse.
- el.createPoint( x, y ) // create an svg point, useful for using later in svg transforms
- el.getViewBox() // getViewbox even if not set (is this duplicate of method below ?)

- el.viewBoxZoom(pt, factor ) // zoom the svg, pt is an svg point, ie from createPoint()

- el.getCursorPoint ( x, y ) // get the cursor point and return as svg point accounting for screen matrix inverse

- el.globalToLocal( globalPoint ) // get transform to point from paper, need to go over and explain this better

- el.zoomCenter( factor ) // zoom into the element as a center (need to add svg zoom as well)

- el.elementZoom( svgPoint, factor ) // zoom the element into a point / rephrase

- el.handleScroll( ev ) // handle scrolling

- el.scaleBy( factor ) // scale by an amount
- el.scaleInc( factor ) // Incrementally rotate (be wary in animation would increase by factor every time)
- el.rotateBy( angle ) // rotate by angl
- el.rotateInc( angle ) // incrementally rotate by angle (in animation will inc every interval)
- el.panBy( x, y ) // pan by amount
- el.panInc( x, y ) // incrementally pan by amount
- el.resetStates() // resets transforms for above
- el.storeDragStart( x, y ) // store drag start
- el.getDragStart( x, y ) // get start of drag
- el.getNewPan() // get a new pan thats going to be applied (not sure if tested)
- el.getNewPanString() // as above but as string ?
- el.getOriginalViewbox() // get the original viewbox before animation
- el.getCurrentScale() // returns just x, so needs modification if x,y scale different
- el.getNewScale() // get new scale to be applied
- el.storeNewScale( scale ) // store a new scale which will be applied when updateTransform
- el.storeNewRotate( angle ) // store a new rotation to be applied later
- el.getCurrentRotation() // get existing rotation
- el.getStartRotation() // get starting rotation
- el.resetNewTransforms() // reset transforms that would be applied
- el.addTransform() // add a transform (doesn't overwrite I think)
- el.createNewViewbox() // create a new viewbox from a new pan set

- el.storeInitialTransform() // store existing transform so we keep this when doing a new transform (so relative, not reset)

- el.updateTransform() // updates transforms that have all just been applied above

- el.pinchmove( event ) // pinchmove an element...worked with Hammer.js I think ?

- el.getViewBoxInfo // get viewbox info (even if not set)


- el.animateFunc( from, to, func, timer, easing, callback ) // animation func that ties into other stuff here, need to doc how this slightly varies from Snaps animate 
- el.animateSvgFocus( duration, easing, callback ) // animate/highlight the Svg/paper element
- el.svgFocus() // zoom into the zvg so its centered

- el.highlightBBox() // highlight bbox around item (you should be able to  el.remove() it after)
 
- el.getTransformedBB() // get the transformed Bounding box

- el.addMouseWheelHandler() // handle scrolling

- el.createHandles() // create freetransform handles
- el.getHandles()    // get the handles
- el.updateHandlePosition() // update handles if you are animating or something
- el.removeHandles() // To be done Important!

- el.markupMpath( markupObj )
- el.markupAnimateMotion( markupObj )
- el.markupAnimate( markupObj ) // SVG markup for SMIL animation (not sure on browser compatilbility)
- el.markupAnimateTransform( markupObj ) // As above
- el.markupAnimateColor( markupObj ) // Not sure if Firefox supports SMIL animateColor ?
```
var mpath = paper.markupMpath({ "xlink:href": "#path1" });
r.markupAnimateMotion( { dur: "5s", fill: "freeze" }, mpath );
r.markupAnimate( { attributeName: "opacity", from: "1", to: "0", dur: "5s", repeatCount: "indefinite" });
```

Example using with Hammer.js

```
var mc = new Hammer( myImage.node, {} );
var pinch = new Hammer.Pinch();
var pan = new Hammer.Pan();
mc.add( pinch );
mc.add( pan );
mc.on('pinchstart', function(ev){
        myImage.resetStates( ev );
} );

mc.on('pinchmove', function(ev) {
        ev.preventDefault();
        myImage.pinchMove( ev );
});

mc.on('pinchend', function(ev) {
});

mc.on('panstart', function( ev ) {
        myImage.onPanStart( ev );
});

mc.on('panmove', function( ev ) {
        ev.preventDefault();
        Snap( ev.target ).panMove( ev );
});
```

