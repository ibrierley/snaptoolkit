## Snaptoolkit

## A toolkit of stuff relating to Snap.svg

This is less of a release, and more of a toolkit of possible solutions to common problems that appear in Snap that people could use and abuse.

## Methods

- el.animateFrames( arrayOfAnimations ) // sequences of animations in order, using callbacks after each
```
c.animateFrames([
    { animation: { transform: 's0.4,0.4,400,100' },         dur: 1000, easing: mina.bounce },
    { animation: { transform: 't50,500' },                dur: 1000, easing: mina.bounce },
    { animation: { transform: 's1.2,1.2,300,300t200,-100' },dur: 1000, easing: mina.bounce }
]);
```

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
