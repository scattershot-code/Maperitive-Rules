# Maperitive Rules for laser cutting

For those that are obsessed by maps, here’s how to get contour data from Open Street Maps and prep it ready for laser cutting on Trotec equipment.

Firstly, you’ll need to download [Maperitive](http://maperitive.net/). ([direct download](http://maperitive.net/download/Maperitive-latest.zip)) You can upzip it and put it on your desktop and run it from there.

First thing you will need to do is create a new rule. Go into the *Maperitive folder > Rules* and duplicate _Default.mrules._ Feel free to change it to *contours.mrules* and open it into a text editor.

Find the lines:
```
contour major : contour[@isMulti(elevation, 100)]
contour minor : contour[@isMulti(elevation, 20) and not @isMulti(elevation, 100)]
```
and change to:
```
contour major : contour[@isMulti(elevation, 100)]
contour minor : contour[@isMulti(elevation, 1) and not @isMulti(elevation, 100)]
```
This sets the elevation pulled from the data to 1 metre.

Save your file and then make sure that Maperitive can find it. Go to the Command Prompt at the bottom of the Maperitive page and type:
```
use-ruleset location=Rules/contours.mrules
```
and then:
```
apply-ruleset
```

### Scripting contours
Next you have to write a script that collects the relevant data, displays it and saves it to a file location. Copy and paste the following into your text editor:
```
// This script will produce a contour map of the area selected by
// the geometery and printing bounds selected
// Made by DMK, 2017
// Adds relief shading so contours can be seen clearer

generate-relief-igor color=gray

// generate contours (display will adjust to zoom level, see next)
// For laser cutting, make sure not to have too many contours

generate-contours interval=5

// This is the distance between contours in metres.
// Optional. Kill the bitmaps. In principle it should be the first map source in the list

remove-source index=1

// Export using a scale. Do not set a zoom here, unless you want to override the zoom map scale.

export-svg file="output/contours/contour-map.svg" compatibility=Inkscape scale=3 copy-images=true
```
Save this as extract-contours.mscript (or whatever you like) in *Maperitive > Scripts*.

Select the area you want the contours for through the *Map > Select Geometry Bounds* and *Map > Select Printing Bounds*. This automatically sets it the size of the screen so zoom out a little and move the corner markers so the geometry and the printing area is the same over the area you want.

Next, select menu *Tools > Generate Relief Contours* and wait for it to finish.

Then run your *extract-contours.mscript* by going to *File > Run Script…* finding it and hitting Open.

Once the script has run, you should see a screen showing the contours of your selected area. Also, if you look in *Maperitive > output > contours* there will be a file called *contour-map.svg* that you can open in CorelDRAW (or whatever) and clean up ready for printing. Each part of the map will be vectorised and in well-labelled layers.

Enjoy.
