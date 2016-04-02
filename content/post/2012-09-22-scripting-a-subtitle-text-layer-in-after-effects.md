---
title: Scripting a subtitle Text layer in After Effects
author: michael
layout: post
date: 2012-09-22
url: /2012/09/scripting-a-subtitle-text-layer-in-after-effects/
categories:
  - Code
tags:
  - After Effects
  - JSX

---
How to use a JSX script to create a text layer that reads subtile keys from a file and creates the keys at the appropriate timecode.

This will use the currently selected Layer (which should be a Text Layer) and reads a JSON string from a text file, containing the subtitle timecodes and texts.

A sample subtitle file looks like this:

    var subs = {
    "00:00:07:07" : "Test Subtitle",
    "00:00:09:07" : "Mehr Text",
    "00:00:12:17" : "Viel Text Viel Text Viel Text...",
    "99:99:99:99" : "Dummy"
    }


This is the JSX code to be run as an AE script:

    $.evalFile("file://c:\\testsub.txt");
    var activeItem = app.project.activeItem;
    var sourceText = activeItem.selectedLayers[0].sourceText;
    //var currentTime = timeToCurrentFormat(t = time + thisComp.displayStartTime, fps = 1.0 / thisComp.frameDuration, isDuration = false, ntscDropFrame = thisComp.ntscDropFrame)

    for (var sub in subs)
    {
    {
            var timeAsSeconds = currentFormatToTime(formattedTime = sub,  fps = 1.0 / activeItem.frameDuration,  isDuration = false);
            sourceText.setValueAtTime(timeAsSeconds, subs[sub]);
    }
    }
