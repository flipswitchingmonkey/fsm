---
title: 'Cinema 4D: Incremental Save using Python'
author: michael
layout: post
date: 2015-03-17
url: /2015/03/cinema-4d-incremental-save-using-python/
categories:
  - Cinema 4D
tags:
  - Cinema4D
  - Python

---
Here&#8217;s a little script for you to use to implement an incremental document save that takes into account letters following the version number, if your naming convention calls for it. For example, here we use a convention using this sort of naming:

  * file_v001_AB.c4d
  * file_v002_YZ.c4d
  * &#8230;

Why? Because we like to sort by version number and this way the files still sort correctly in the explorer/finder despite artist initals.

Anyway, here&#8217;s the script:

    import c4d
    import os
    from c4d import gui
    
    # Saves the current document with incremented version number
    
    
    def IncreaseVersion(fullPath):
        fn, ext = os.path.splitext(fullPath)
        fnreverse = fn[::-1]  # reverse string
    
      versionString = ""
      versionNumber = 0
      beginVersionString = False
      for letter in fnreverse:
          if letter.isdigit() is True:
              beginVersionString = True
              versionString += letter
              print("Digit: {0}").format(letter)
          else:
              print("Non-Digit: {0}").format(letter)
              if beginVersionString is True:
                  versionString = versionString[::-1]  # reverse string
                  versionNumber = int(versionString)
                  break  # first non-digit ends version string
    
      versionNumber += 1
      versionStringNew = str(versionNumber).zfill(len(versionString))
    
      fnreverse = fnreverse.replace(versionString[::-1], versionStringNew[::-1], 1)
      fullPathNew = fnreverse[::-1] + ext
    
      print("Increased version {0} to {1}").format(versionString, versionStringNew)
      print("Changed path {0} to {1}").format(fullPath, fullPathNew)
      return fullPathNew
    
    
    def main():
      currentPath = doc.GetDocumentPath()
      currentName = doc.GetDocumentName()
      fullPathNew = IncreaseVersion(currentPath + os.sep + currentName)
      newPath, newName = os.path.split(fullPathNew)
    
      doc.SetDocumentPath(newPath)
      doc.SetDocumentName(newName)
      if c4d.documents.SaveDocument(doc, fullPathNew, saveflags=c4d.SAVEDOCUMENTFLAGS_0, format=c4d.FORMAT_C4DEXPORT):
          print("Saved as {0}").format(fullPathNew)
    
      else:
          print "Error whilst saving." + newPath
          gui.MessageDialog('Error whilst saving to ' + newPath)
          doc.SetDocumentPath(currentPath)
          doc.SetDocumentName(currentName)
    
    
      if __name__=='__main__':
          main()
    

And, if you like, an icon too: 

 <img src="/uploads/2015/03/MA_saveIncremental.jpg" style="width:32px;height:32px;" />  
[JPG file](/uploads/2015/03/MA_saveIncremental.jpg)  
[TIF file](/uploads/2015/03/MA_saveIncremental.tif)