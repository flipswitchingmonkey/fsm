---
title: 'Cinema 4D and After Effects: OpenEXR Multi-Layer workflow'
author: michael
layout: post
date: 2015-03-27
url: /2015/03/cinema-4d-and-after-effects-openexr-multi-layer-workflow/
categories:
  - Cinema 4D
tags:
  - After Effects
  - Cinema4D

---
What follows is a simple step by step guide to set up your project for OpenEXR output in Cinema 4D in a way that makes sure the linear workflow is kept intact from the render to the compositing in After Effects (or other similar apps).

# Cinema 4D {#cinema4d}

##### Project setup {#projectsetup}

![](/uploads/2015/03/OpenEXR_Workflow_0010.png)
  
Open your Project Settings either through the menu Edit > Project Settings&#8230; or by pressind Ctrl-D (for OSX it&#8217;s usually the same shortcut, only it&#8217;s Cmd instead of Ctrl). Here, make sure that Linear Workflow is checked and that the Input Color Profile is set to sRGB. This should be the default setting in Cinema 4D these days, but it&#8217;s better to check and be sure.

Background (skip if you don&#8217;t care): Linear workflow refers to the absence of gamma correction from you color workflow. Right. So what is gamma correction about. Our eyes read brightness not in a linear fashion. A doubling of light does not mean a doubling of brightness. What appears as 50% grey to us is actually just 18% grey. Gamma correction takes the fact that we recognize brightness on a curve into account and encodes colors accordingly, so we have more shades of those colors available where we actually notice the difference, and fewer where we wouldn&#8217;t notice anyway. This is great from a visual perspective (quite literally so), but not so much from a computer&#8217;s or camera sensors. Neither really care for our eyes&#8217; lack of linear reception. And having to constantly add and substract gamma curves can cause all kinds of color shifts, even more so when there are slight variations in the methods these are applied or even worse, different gamma values. So what is being done instead is to internally treat all color linear, without gamma. 50% grey is 50% grey, everywhere. And thanks to 32 bit value ranges we have plenty of room for pretty much every color value, unlike in the days of 8 bit where we had 256 grey values and that was that. So, to get back to the project settings, what we set up here is this: we tell Cinema 4D to internally calculate everything in a linear fashion (Linear Workflow checked on), but to also expect textures and stuff coming from outside to be using the sRGB profile (which is the linear color multiplied by a gamma curve of 2.2) or whatever other profile is embedded inside the image file. This means you can work in Photoshop etc. as usual, save your sRGB based images (which is what you mostly work in when you work on a computer monitor) and give them over to Cinema 4D without thinking about color profiles. Beware though: if you use HDRI for lighting, those usually come as 32bit HDR or EXR files that are linear and not sRGB! So for those you have to make sure their profile is set to linear (which usually Cinema 4D does automatically).

##### Textures {#textures}

![](/uploads/2015/03/OpenEXR_Workflow_0020.png)

JPG, PNG&#8230; textures are usually encoded with sRGB. Since this is the default settings, nothing should need to be done. In 99% of all cases the fact that the file is using sRGB is embedded in the metadata and the default &#8220;Color Profile&#8221; setting of &#8220;Embedded&#8221; will recognize this.

Same goes for EXR, which are always linear.

If in doubt check the preview image. The wrong color profile will almost always look very significantly off.

Little tip: right click on the preview image and click on &#8220;Open Window&#8230;&#8221;. It&#8217;ll open the preview in a separate, scalable window that&#8217;ll let you render larger previews.

##### Render Settings {#rendersettings}

![](/uploads/2015/03/OpenEXR_Workflow_0030.png)

There are a few things to look out for in the render settings.

Obviously Save and Multi-Pass should be checked, with all needed passes added to the Multi-Pass. Since we want everything in a single multi-layer OpenEXR file, make sure to also add an RGBA Image pass to your multi-pass and to disable the Save under Regular Image. Otherwise you get an additional file with another Beauty pass, which just clutters up the folder and isn&#8217;t needed.

Make sure under Multi-Pass-Image you changed the Format to OpenEXR. It&#8217;s always 32bit. Check the Multi-Layer File box, otherwise you get one separate EXR file per pass. Layer Name as Suffix does not have any effect on multi layered files, but leave it checked on just in case. Straight Alpha should be left on if you need to composite the render on top of something else. It&#8217;ll give you a clean alpha channel that is not tinted by the background, unlike a premultiplied alpha. Use Premultiplied (Straight alpha unchecked) only if you know exactly what the background color will be and you rendered the image on the very same background.

Important: if, like in the screenshot, the Regular Image file format was set to something other than OpenEXR, the Image Color Profile was probably defaulting to sRGB. Even though it is not obvious, the Regular Image&#8217;s Image Color Profile will affect the Multi-Pass Image too! Thus, it needs to be set to Linear Color Space manually! Otherwise you end up with sRGB embedded into your EXR and have to manually counteract that again in After Effects, causing all kinds of confusion&#8230;

##### Result {#result}

If all went well we should now end up with a single file EXR that contains all the passes, is compressed and uses linear gamma. This is perfect for compositing for several reasons. As explained, linear workflow prevents color shifts between applications or even inside a single application. A single file per frame also prevents chaos, where otherwise for every frame there would be 20 files in a folder (making it 20.000 in a 1000 frame sequence). And also the compositing app only needs to cache and access a single file as compared to many, which can make a big difference over a network.

# After Effects {#aftereffects}

##### Project setup {#projectsetup}

Just like in Cinema 4D, your first action should be to set up the project correctly. Go to File > Project Settings&#8230; and under Color Settings change the Working Space to sRGB IEC61966-2.1 and check the Linearize Working Space box. Now we have basically the same setup that we have in Cinema 4D. Ideally you could now also work in 32 bits per channel depths, but this is very memory intensive and may slow things down quite a bit. It shouldn&#8217;t matter now (except for some Layer Effects that behave differently in different depths! Trial and error&#8230;.)

##### Interpret Footage {#interpretfootage}

![](/uploads/2015/03/OpenEXR_Workflow_0040.png)
  
Import the EXR sequences as normal. Then right click and Interpret Footage. For some reason AE never guesses the Alpha right on a straight alpha EXR, so change that to Straight &#8211; Unmatted. (in the screenshot the Frame Rate is greyed out because it was a single still image, usually you&#8217;d have to set the correct frame rate here)

![](/uploads/2015/03/OpenEXR_Workflow_0050.png)
  
More important though are the Color Management settings. Assign the sRGB IEC61966-2.1 profile and change the Interpret As Linear Light to On (not just for 32 bit).

If you add non-linear files (files that use sRGB encoding for example) to the project, set everything up as above but change the Interpret As Linear Light to Off (which AE should do by default).

##### Accessing the Layers in the EXR {#accessingthelayersintheexr}

Accessing the layers in a multi-layer EXR is being done through the &#8220;EXtractoR&#8221; effect, which is basically a stripped down version of the [ProEXR plugin](http://www.fnordware.com/ProEXR/) that comes with AE. You can find it under Effect > 3D Channel > EXtractoR. Add the EXR to your comp and add the EXtractoR effect to the layer like so:
  
![](/uploads/2015/03/OpenEXR_Workflow_0055.png)

Click not where it tells you to click (I know&#8230;) but below, where it lists the Red/Green/Blue channels. A new dialog pops up.
  
![](/uploads/2015/03/OpenEXR_Workflow_0056.png)
  
Under Layers you should see all your multi-pass renders. In the case of RGB passes like Reflection, Diffuse etc. the dialog will fill out the Red, Green, Blue and Alpha channel automatically with the correct layer. Object Buffers however only have a single color channel and this trips up the dialog. No worries though, just set Red, Green, Blue to the Object Buffer&#8217;s Y channel (Y for luminance). Leave the alpha on (copy), since the Object Buffers don&#8217;t have an alpha channel (they are full frame greyscale where black = alpha). 

Duplicate the layer as many times as required to get all your passes. Now you have every pass on a layer, but AE only has to read a single file for all those.

##### Result {#result}

This should be all, things should now look correctly from render to output! But don&#8217;t trust my word for it.

Here&#8217;s how the image looked in the Cinema 4D picture viewer:
  
![](/uploads/2015/03/OpenEXR_Workflow_0060.png)

And here&#8217;s the same in AE:
  
![](/uploads/2015/03/OpenEXR_Workflow_0070.png)

And here&#8217;s a screenshot from an MP4 generated from that same comp:
  
![](/uploads/2015/03/OpenEXR_Workflow_0080.png)

Looks the same to me. Success :)

Further reading:

  * <http://www.cambridgeincolour.com/tutorials/gamma-correction.htm>
  * <http://throb.net/wp-content/uploads/2009/10/Linear_workflow.pdf>
  * <http://helloluxx.com/tutorials/cinema4d-2/cinema4d-rendering/linear-workflow-in-cinema4d-and-after-effects/>
  * and so on
