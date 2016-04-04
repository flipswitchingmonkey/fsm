---
title: A toolbar for After Effects
author: michael
layout: post
date: 2016-03-30
categories:
  - Code
tags:
  - AfterEffects
  - ExtendScript
  - JavaScript
image: "2016-04-04-a-toolbar-for-after-effects.png"
---

I'm currently busy writing a bunch of After Effects scripts (in ExtendScript, Adobe's Javascript dialect) and it's rather painful to have to dig through
a long list of scripts under  File > Scripts > something or fire up the debugger every time. How handy would it be to, you know, just click a button on
a customizable toolbar, like pretty much every other software offers. Pretty handy you say? Yes. Only Adobe has not deemed it necessary so far (just like
it is still a pain to open image sequences if you don't own the Immigration script. Seriously, Adobe, why?).

Anyway, so much for the introduction. Here's a small ScriptUI script that you may use as the base of your own implementation, if you like, or just us as-is,
if it works for you.

Disclaimer: this is a script I made for myself, for internal use for me and my colleagues at [908video](http://www.908video.de). So there is very little
going on in terms of security or error checking (just the basics). If you plan to use it in a larger setting, be aware that evaluating any old Javascript
that you read from some XML file WILL eventually bite you in the ass. Consider yourself warned. That said, here we go. I'll discuss the script for a bit, but
the whole thing can be found Gitlab: [https://gitlab.com/flipswitchingmonkey/ma_toolbar](https://gitlab.com/flipswitchingmonkey/ma_toolbar)


    /*
        ma_toolbar
        Michael Auerswald <michael@flipswitchingmonkey.com>
    */

    // Global object
    // encapsulates all other variables "global" to this script 
    var ma_toolbar = {};

So this is actually quite important. Make sure to always wrap your globals into a variable, because all AE scripts are running in the same namespace!
And chances are good some other script write has used i, x, debug or whatever commong variable name for something and this will cause all kinds of
weird and wonderful bugs. 

    // this should be the path to the folder containing the script and icons folder
    ma_toolbar.scriptPath = (new File($.fileName)).path;
    
`(new File($.fileName)).path;` is a nice little workaround to give you the current path of the script that you are executing. From there we can extrapolate
the path to other scripts and stuff. Again, this works for me since I have all the scripts under the Scripts folder, but if for some reason yours are elsewhere,
you will either have to do some magic with relative pathes are work with absolutes somehow (but then remember to store one for Win and one for OSX).

    ma_toolbar.resourcePath = (ma_toolbar.scriptPath + "/");
    ma_toolbar.iconPath = (ma_toolbar.resourcePath);
    ma_toolbar.settingsPath = (ma_toolbar.scriptPath + "/ma_toolbar.xml");
    ma_toolbar.iconSize = 32;
    ma_toolbar.iconMargin = 0;
    ma_toolbar.tools = new Array();
    ma_toolbar.xml = new XML();

Small helper function to determine if this is Windows or OSX:

    ma_toolbar.isWin = false;
    if ( ($.os).search(/windows/i) != -1) ma_toolbar.isWin = true; // a few events are triggered differently on windows vs osx

Newlines in AE are stored as \r on OSX and \r\n on Windows:

    ma_toolbar.newLine = '\r';
    if (ma_toolbar.isWin) ma_toolbar.newLine = '\r\n';

As the comment says: there's no proper Replace All function. Now there is.

    // since Javascript does not offer a full replaceAll function for String, this is a replacement
    String.prototype.replaceAll = function(search, replace)
    {
        //if replace is null, return original string otherwise it will
        //replace search string with 'undefined'.
        if(!replace) 
            return this;

        return this.replace(new RegExp('[' + search + ']', 'g'), replace);
    };

    // object type to contain a Tool definition for each button to appear on the toolbar
    ma_toolbar.Tool = function() {
        this.label = "";
        this.path = "";
        this.icon = "";
        this.iconlabel = "";
        this.id = 0;
        this.script = ""
        this.width = 32;
    }

ExtendScript comes with its own XML reader which actually works pretty well. The syntax is some old-timish way to access XML data using
the E4X protocol that iirc never made it into a standard. There's some obsolete documentation to be found on the Mozilla Dev Network
though: [https://developer.mozilla.org/en-US/docs/Archive/Web/E4X_tutorial](https://developer.mozilla.org/en-US/docs/Archive/Web/E4X_tutorial) 

    ma_toolbar.Tool.prototype.fromXML = function(xml, id) {
        this.label = ma_toolbar.xml.tool[i].@label;
        this.path = ma_toolbar.scriptPath + "/../" + ma_toolbar.xml.tool[i].@path;
        if (!(new File(this.path)).exists)
        {
            alert("Script " +  this.label + " not found");
        }
        this.script = ma_toolbar.xml.tool[i].text();
        var iconname = ma_toolbar.xml.tool[i].@icon;
        this.icon = ma_toolbar.iconPath + iconname;
        if (!(new File(this.icon)).exists || iconname=="")
        {
            this.icon = undefined;
            this.iconlabel = iconname.toString();
        }
        this.width = ma_toolbar.xml.tool[i].@width;
        if(id) this.id = id;
    }

This function reads the config data from an XML file.

    function readSettingsFromXML() {
        var xmlFile = new File(ma_toolbar.settingsPath);
        var result = undefined;
        var xml;
        if (!xmlFile.exists)
        {
            alert("Can not find settings file - please make sure you installed the script correctly.");
            quit();
        }
        else
        {
            result = xmlFile.open("r");
            if (result)
            {
                var xmlFileContent = xmlFile.read();
                xmlFile.close();
                xml = new XML(xmlFileContent);
            }
            else
            {
                alert("Can not open preset file - please make sure you installed the script correctly.");
                quit();    
            }
        }
        return xml;
    }

This is the part that is security critical. As in, there is none. Scripts and files are just executed without further checks (other than checking whether
they actually exist). So if you plan to use this in production, this would be a good place to make changes for the better.

    function runCommand(cmdId)
    {
        try{
            if(ma_toolbar.tools[cmdId].script == "")
            {
                $.evalFile(ma_toolbar.tools[cmdId].path);
            }
            else
            {
                aftereffects.executeScript(ma_toolbar.tools[cmdId].script);
            }
        } catch(err) {
            alert(err.message + "\nCould not launch " + ma_toolbar.tools[cmdId].label + " from path " + ma_toolbar.tools[cmdId].path);
        }
    }

Here we create the actual panel. It's creating one button horizontal or vertical based on the previously read XML file. A good improvement would be to automatically create
multiple rows of icons etc.

    function createUI(thisObj)
    {
        var dlg = (thisObj instanceof Panel) ? thisObj : new Window("palette", "ma-toolbar", undefined, {resizeable:true});
        dlg.orientation = "column";
                
        for (var i=0; i < ma_toolbar.xml.tool.length(); i++)
        {
            var scriptBtn;
            var width = (ma_toolbar.tools[i].width=="") ? ma_toolbar.iconSize : ma_toolbar.tools[i].width; 
            if(ma_toolbar.tools[i].icon)
                scriptBtn = dlg.add("iconbutton", "x:"+ma_toolbar.iconMargin+",y:"+ma_toolbar.iconMargin+"+,width:"+width+",height:"+ma_toolbar.iconSize, ma_toolbar.tools[i].icon, {style: 'toolbutton'});
            else
                scriptBtn = dlg.add("button", "x:"+ma_toolbar.iconMargin+",y:"+ma_toolbar.iconMargin+"+,width:"+width+",height:"+ma_toolbar.iconSize, ma_toolbar.tools[i].iconlabel, {style: 'toolbutton'});
            scriptBtn.ID = i;
            scriptBtn.helpTip = ma_toolbar.tools[i].label;
            scriptBtn.onClick = function(){
                runCommand(this.ID);
            }
        }
        dlg.onResizing = dlg.onResize = function () {
            this.layout.resize();
                if (this.size[0] < this.size[1])
                {
                    dlg.orientation = "column";
                }
                else
                {
                    dlg.orientation = "row";
                }
            }            
    
        return dlg;
    }

    ma_toolbar.xml = readSettingsFromXML();
    ma_toolbar.tools = new Array(ma_toolbar.xml.tool.length());
    for (var i=0; i < ma_toolbar.xml.tool.length(); i++)
    {
        ma_toolbar.tools[i] = new ma_toolbar.Tool();
        ma_toolbar.tools[i].fromXML(ma_toolbar.xml.tool[i], i);
    }    

    ma_toolbar.dlg = createUI(this);
    if (ma_toolbar.dlg instanceof Window) ma_toolbar.dlg.show();
    

Thats's the script. Drop it into your AE's Scripts/ScriptUI folder and restart AE. It then should show up under your Window menu.

The config file looks like this:

    <!--
    Tool definition file for ma_toolbar

    Define a new tool by adding a new tool element like this:
    <tool path="myscript/myscript.jsx" name="My Script" icon="myicon.png"/>

    Parameters:
    path: the script's file path relative to the After Effects "Scripts" folder. Use forward slashes to be OS-independent
    label: an arbitraty name you give to the script
    icon: an icon file placed under the res/icons/ folder, 32x32 pixels in size, or a short text to label the button
    width: optional parameter, width of button (defaults to 32)

    To add an AE expression instead of a file, leave out the path parameter and instead write your script as text between the <tool></tool> tags.
    -->

    <tools>
        <tool path="ScriptUI Panels/Immigration.jsxbin" label="Immigration" icon="Immi" width="32"/>
        <tool path="ScriptUI Panels/BG Renderer.jsxbin" label="BG Renderer" icon="BG"/>
        <tool path="rd_scripts/rd_GimmePropInfo.jsx" label="rd_GimmePropInfo" icon="Prop"/>
        <tool path="rd_scripts/rd_SimpleConsole.jsx" label="rd_SimpleConsole" icon="Con"/>
        <tool label="Test Script" icon="icons/default.png">
            app.executeCommand(app.findMenuCommandId("About After Effects..."));
        </tool>
    </tools>
    
Well the commment basically says it already. The paths are all relative to the Scripts folder in your AE installation. Label is being used for the MouseOver Hint message. 
The icon can either be a filename (to be found under the ma_toolbar.iconPath) or a string which will be the button label. Anything over four characters probably won't fit
your 32px button though. The Test Script entry shows what an after effects script or expression would look like. Just leave the path parameter and instead put the script between
the tags.

I hope this will be of use to someone - if so it'd be nice to hear some feedback. If not, well, I'll never know (and that's fine). 