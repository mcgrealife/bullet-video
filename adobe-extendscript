
// Change the values of these 3 variables to change the font.
//========================================================
var subsHeadingFont = "Lato-Bold";
var subsHeadingFontSize = 49;

var subsPointFont = "Lato-Regular";
var subsPointFontSize = 38;

var subsSubPointFont = "Lato-Regular";
var subsSubPointFontSize = 33;
//========================================================


// Half-screen extention time. This variable decides how long the half-screen remains on the screen after the subtitles.
//========================================================
var halfScreenExtendTime = 2;
//========================================================

// Logo dimensions. [width, height].
//========================================================
var logoDimens = [372, 100];
//========================================================

// Change this value to change the X-Coordinate of the bullet-point
//========================================================
var overridePointBulltetPointVal = 0;
//========================================================

var proj = app.project; 
var settingsFile = new File(Folder.current.absoluteURI + "/Video-Creator-Settings.txt");
var templateProjName = "Video-Creator-Template.aep";

var vidTitle = "";
var vidDescription = "";

var logoArray = [];
var commercialsArray = [];
var infoVidFolder = null; // Folder containing all the Main videos and their respective subtitle AE files
var curInfoVidFile  = null;
var curComVidFile = null;
var curLogoFile = null;
var introFile = null;
var outroFile = null;
var spreadsheetFile = null;
var ameWatchFolder = null; // Adobe Media Encoder watch folder

var vidDimens = [1920,1080]; // Internal video dimensions
var finalVidDimens = vidDimens; // Final video dimensions
var videoBGColor = [206/255, 206/255, 206/255];
var introComIndentTime = 0.5; // How much the commerical video needs to be indented into the intro video (measured in seconds)
var comTransIndentTime = 0.5; // How much the transition video needs to be indented into the commercial video (measured in seconds)
var transInfoVidIndentTime = 1.5; // How much the info video needs to be indented into the transition video (measured in seconds)
var infoVidOutroIndentTime = 1; // How much the outro video needs to be indented into the info video (measured in seconds)
var halfScreenPaddingTime = halfScreenExtendTime; // The time in-between the half-screen moving-in and the text start appearing.
var comInfoVidTransComp = null; // Transition comp between commerial and infor video.
var infoVidItem = null; // Imported info video project item
var infoVidLayer = null; // Info video layer in the main comp
var nullMarkerLayer = null; // Subtitle null marker layer
var infoVidCenterX = 960; // x-coord of the center of person in info video 
var stopVideoCreation = false; // Stops the video creation process after the current video


// UI Elements
var txtIntro = null; // Intro file path textbox
var txtOutro = null; // Outro file path textbox
var txtInfoVid = null; // Main video folder path textbox
var txtSpreadsheet = null; // Spreadsheet file (should be CSV)
var txtVidWidth = null; // Video width textbox
var txtVidHeight = null; // Video height textbox
var txtWatchFold = null; // Adobe Media Encoder watch folder textbox
var txtColor = null; // Background color hex code

function createUI ()
{
    var userPanel = new Window ("dialog", "Video Creator - V4.8", undefined); 
    var introMainGroup =  userPanel.add("group", undefined, "Intro Main Group");
    introMainGroup.orientation = "column";
     
        var lblIntro = introMainGroup.add("statictext", undefined, "Intro video:");
        lblIntro.alignment = "left";
        var introInGroup = introMainGroup.add("group", undefined, "Intro Input Group");
            txtIntro = introInGroup.add("edittext", [0,0,150,22], "");
            var btnIntro = introInGroup.add("button", undefined, "Browse");
            btnIntro.onClick = function(){selectIntroFile ()};

    var outroMainGroup = userPanel.add("group", undefined, "Outro Main Group");
    outroMainGroup.orientation = "column";
    
        var lblOutro = outroMainGroup.add("statictext", undefined, "Outro video:");
        lblOutro.alignment = "left";
        var outroInGroup = outroMainGroup.add("group", undefined, "Outro Input Group");
            txtOutro = outroInGroup.add("edittext", [0,0,150,22], "");
            var btnOutro = outroInGroup.add("button", undefined, "Browse");
            btnOutro.onClick = function(){selectOutroFile()};

    var infoVidGroup = userPanel.add("group", undefined, "Info Video Group");
    infoVidGroup.orientation = "column";
    
        var lblInfoVid = infoVidGroup.add("statictext", undefined, "Main video folder:");
        lblInfoVid.alignment = "left";
        var infoVidInGroup = infoVidGroup.add("group", undefined, "Info Video Input Group");
            txtInfoVid = infoVidInGroup.add("edittext", [0,0,150,22], "");
            var btnInfoVid = infoVidInGroup.add("button", undefined, "Browse");
            btnInfoVid.onClick = function(){selectInfoVideoFolder()};

    var spreadSheetGroup = userPanel.add("group", undefined, "Spread Sheet Group");
    spreadSheetGroup.orientation = "column";
    
        var lblSpreadSheet = spreadSheetGroup.add("statictext", undefined, "Spreadsheet (.csv):");
        lblSpreadSheet.alignment = "left";
        var ssInGroup = spreadSheetGroup.add("group", undefined, "Spreadsheet Input Group");
            txtSpreadsheet = ssInGroup.add("edittext", [0,0,150,22], "");
            var btnSpreadsheet = ssInGroup.add("button", undefined, "Browse");
            btnSpreadsheet.onClick = function(){selectSpreadsheet()};
    
    var watchFoldGroup = userPanel.add("group", undefined, "Watch Folder Group");
    watchFoldGroup.orientation = "column";   
    
        var lblWatchFold = watchFoldGroup.add("statictext", undefined, "Adobe Media Encoder watch folder:");
        lblWatchFold.alignment = "left";
        var watchFoldInGroup = watchFoldGroup.add("group", undefined, "Watch Folder Input Group");
            txtWatchFold = watchFoldInGroup.add("edittext", [0,0,150,22], "");
            var btnWatchFold = watchFoldInGroup.add("button", undefined, "Browse");
            btnWatchFold.onClick = function(){selectVidWatchFolder()};
            
    var advancedPanel = userPanel.add("panel", undefined, "Advanced");
        var vidDimensGroup = advancedPanel.add("group", undefined, "Video Dimensions Group");
        vidDimensGroup.orientation = "column";
        vidDimensGroup.alignment = "left";
        
            var lblVidDimens = vidDimensGroup.add("statictext", undefined, "Video Dimensions (WIDTH x HEIGHT):");
            lblVidDimens.alignment = "left";
            var  vidDimensInGroup = vidDimensGroup.add("group", undefined, "Video Dimensions Input Group");
            vidDimensInGroup.alignment = "left";
                txtVidWidth =  vidDimensInGroup.add("edittext", [0,0,50,22], "1920");
                var lblMultiply = vidDimensInGroup.add("statictext", undefined, "x");
                txtVidHeight =  vidDimensInGroup.add("edittext", [0,0,50,22], "1080");

        var colorGroup = advancedPanel.add("group", undefined, "Color Group");
        colorGroup.orientation = "column";
        colorGroup.alignment = "left";
            var lblColor = colorGroup.add("statictext", undefined, "Background Color (Hex Code):");
            lblColor.alignment = "left";
            txtColor = colorGroup.add("edittext", [0,0,50,22], "CECECE");
            txtColor.alignment = "left";

    var btnStart = userPanel.add("button", [0,0,100,30], "Start");
    btnStart.onClick = function(){startUp ()};
    var btnStop = userPanel.add("button", [0,0,100,30], "Close");
   btnStop.onClick = function(){shutdownScript ()};
    
   return userPanel;      
}

// The GUI is created, centered and displayed.
var controlPanelGUI = createUI();
grabFilePaths ();
controlPanelGUI .center(); 
controlPanelGUI .show();

function grabFilePaths ()
/*
* This function checks whether the settings file is present and if so, opens it and imports
* the previously selected settings.
*/
{
    if (settingsFile.exists)
    {
        var fileOpened = settingsFile.open('r');
        if (fileOpened)
        {
            infoVidFolder = new Folder(settingsFile.readln());
            txtInfoVid.text = decodeURI(infoVidFolder.absoluteURI)
            introFile = new File(settingsFile.readln());
            txtIntro.text = decodeURI(introFile.absoluteURI);
            outroFile = new File(settingsFile.readln());
            txtOutro.text = decodeURI(outroFile.absoluteURI);
            spreadsheetFile = new File(settingsFile.readln());
            txtSpreadsheet.text = decodeURI(spreadsheetFile.absoluteURI);
            ameWatchFolder = new Folder(settingsFile.readln());
            txtWatchFold.text = decodeURI(ameWatchFolder.absoluteURI);
            txtVidWidth.text = decodeURI(settingsFile.readln());
            txtVidHeight.text = decodeURI(settingsFile.readln());
            txtColor.text = decodeURI(settingsFile.readln());
            settingsFile.close();
        }
    }
}

function selectIntroFile ()
/*
* Allows the user to select the Intro video file.
*/
{
    var tmpIntro = File.openDialog("Please select the intro video");
    if (tmpIntro != null)
    {
        introFile = tmpIntro;
        txtIntro.text = decodeURI(tmpIntro.absoluteURI.toString());
    }
}

function selectOutroFile ()
/*
* Allows the user to select the Outro video file.
*/
{
    var tmpOutro = File.openDialog("Please select the outro video");
    if (tmpOutro != null)
    {
        outroFile = tmpOutro;
        txtOutro.text = decodeURI(tmpOutro.absoluteURI.toString());
    }
}

function selectInfoVideoFolder ()
/*
* Allows the user to select the video folder (containing the videos and the respective subtitle projects).
*/
{
    var tmpInfoVidFold = Folder.selectDialog("Please select the main video folder");
    if (tmpInfoVidFold != null)
    {
        infoVidFolder = tmpInfoVidFold;
        txtInfoVid.text = decodeURI(tmpInfoVidFold.absoluteURI.toString());
    }
}

function selectSpreadsheet ()
/*
* Allows the user to select the spreadsheet file.
*/
{
    var tmpSpreadsheetFile = File.openDialog("Please select the spreadsheet");
    if (tmpSpreadsheetFile != null)
    {
        spreadsheetFile = tmpSpreadsheetFile;
        txtSpreadsheet.text = decodeURI(tmpSpreadsheetFile.absoluteURI.toString());
    }
}
  
function selectVidWatchFolder ()
/*
* Allows the user to select the project output folder (the folder to which the completed projects are saved).
*/
{
    var tmpWatchFold = Folder.selectDialog("Please select the Adobe Media Encoder watch folder");
    if (tmpWatchFold != null)
    {
        ameWatchFolder = tmpWatchFold;
        txtWatchFold.text = decodeURI(tmpWatchFold.absoluteURI.toString());
    }
}

function shutdownScript ()
/*
* Shutsdown the script.
*/
{
    stopVideoCreation = true;
    controlPanelGUI .close();
}

function startUp ()
/*
* This function checks whether all the neccessary user-inputs are present and if so,
* starts the video creation process.
*/
{
    try
    {
        if (infoVidFolder == null)
            alert("Main video folder not selected. Please select the main video folder and try again.");
        else if (introFile == null)
            alert("Intro video not found. Please select a valid intro video and try again.");
        else if (outroFile == null)
            alert("Outro video not found. Please select a valid outro video and try again.");
        else if (spreadsheetFile == null)
            alert("Spreadsheet not found. Please select a valid spreadsheet and try again.");
        else if (spreadsheetFile.name.indexOf(".csv") == -1)
            alert("Invalid spreadsheet. Spreadsheet should be of type: Comma Seperated Value (.csv)");
        else if (ameWatchFolder == null)
            alert("Adobe Media Encoder watch folder not selected. Please select the folder and try again.");
        else if (txtVidWidth.text == "" || txtVidHeight.text == "")
            alert("Please input a valid width and height for the video.");
        else if (txtColor.text =="")
            alert("Please input a valid hex code.");
        else
        {
            var fileOpened = settingsFile.open("w");
            if (fileOpened)
            {
                settingsFile.writeln(infoVidFolder.absoluteURI);
                settingsFile.writeln(introFile.absoluteURI);
                settingsFile.writeln(outroFile.absoluteURI);
                settingsFile.writeln(spreadsheetFile.absoluteURI);
                settingsFile.writeln(ameWatchFolder.absoluteURI);
                settingsFile.writeln(txtVidWidth.text);
                settingsFile.writeln(txtVidHeight.text);
                settingsFile.writeln(txtColor.text);
                
                settingsFile.close();
            }
            
            finalVidDimens = [parseInt(trimWhitespaces(txtVidWidth.text)), parseInt(trimWhitespaces(txtVidHeight.text))];
            if (txtColor.text != "")
                decodeHexCode (trimWhitespaces(txtColor.text));
            mainCtrl();
        }
    }catch(e){
        alert(e);
    }
}
    
    
function mainCtrl()
/*
* This function controls the whole video creation process. It gets all the video files and the respective subtitle projects from the video 
* folder. Each video is iterated against each commercial and each logo in the spreadsheet file. The completed AE project file is stored 
* in the "watch folder".
*/
{
    try
    {
        stopVideoCreation = false;
        decodeSpreadsheet ();
        var allFoldVidFiles = infoVidFolder.getFiles(/\.(mp4|mov|avi|ts)$/i);
        var aepFiles = infoVidFolder.getFiles(/\.(aep)$/i);
        var videoSubtitleArray = []; // Array of arrays. Each inner array pair contains the video and its respective subtitle AE project. 
        
        // matches each video file with its respective comments project
        var m = 0;
        var n = 0;
        for (m=0; m<aepFiles.length; m++)
        {
            var fileNameOnly = aepFiles[m].name.substr(0, aepFiles[m].name.length - 4) // Removes the ".aep" part of file
            for (n=0; n<allFoldVidFiles.length; n++)
            {
                if (allFoldVidFiles[n].name.indexOf(fileNameOnly) != -1)
                    videoSubtitleArray.push([allFoldVidFiles[n], aepFiles[m]]);
            }
        }

        p = 0;
        m =0;
        n = 0;
        for (p=0; p<videoSubtitleArray.length; p++)
        {
            for (m=0; m<logoArray.length; m++)
            {
                for (n=0; n<commercialsArray.length; n++)
                {
                    controlPanelGUI.update();
                    if (stopVideoCreation != true)
                    {
                        app.open(new File(Folder.current.absoluteURI + "/" + templateProjName));
                        proj = app.project;
                        curInfoVidFile = videoSubtitleArray[p][0];
                        curLogoFile = logoArray[m];
                        curComVidFile = commercialsArray[n];
                        convrtProjToFrameIO (videoSubtitleArray[p][1]);
                        vidTitle = trimWhitespaces(nullMarkerLayer.marker.keyValue(1).comment.substring(nullMarkerLayer.marker.keyValue(1).comment.indexOf(": ")+2));
                        vidDescription = nullMarkerLayer.marker.keyValue(2).comment.substring(nullMarkerLayer.marker.keyValue(2).comment.indexOf(": ")+2);
                        theDocName = trimWhitespaces(nullMarkerLayer.marker.keyValue(3).comment.substring(nullMarkerLayer.marker.keyValue(1).comment.indexOf(": ")+2));
                        var finalVidName = theDocName + " " + vidTitle + " (logo-" + m + ")" + "(com-" + n + ")" + " (Render)";
                        
                        createVideo(finalVidName);
                        proj.save(new File(ameWatchFolder.absoluteURI + "/" + finalVidName + ".aep"));
                        proj.close(CloseOptions.DO_NOT_SAVE_CHANGES);
                    }
                }
            }
        }
        alert("Video creation completed! All videos have been successfully created.");
     }catch(e){
        alert(e);
        stopVideoCreation = true;
        controlPanelGUI .close();    
     }
}


function decodeHexCode (colorHexCode)
/*
* This function accepts a (string) color code as its parameter. It converts the color code to its respective RGB values. It stores the RGB values
* as an array in the "videoBGColor" variable.
*/
{
    try{
        var redCol = parseInt(colorHexCode.substr(0,2),16)/255;
        var greenCol = parseInt(colorHexCode.substr(2,2),16)/255;
        var blueCol = parseInt(colorHexCode.substr(4,2),16)/255;
        
        videoBGColor = [redCol, greenCol, blueCol];
    }catch(e){
        alert("Error occrured at: decodeHexCode");
        alert(e);
        throw Error("Unexpected error");
    }
}

function decodeSpreadsheet ()
/*
* The first row of the spreadsheet is ignored. From the second row, the first column represents a logo path while 
* the second column represents a commercial video path. "BLANK" keyword is used to specify no logo or no commercial.
*/
{
    try{
        var  isOpen = spreadsheetFile.open("r");
        if (isOpen)
        {
            spreadsheetFile.readln();
            while (!spreadsheetFile.eof)
            {
                var curLine = spreadsheetFile.readln();
                var logoCommercialArray = curLine.split(",");
                if (logoCommercialArray[0] != "")
                {
                    if (logoCommercialArray[0] == "BLANK")
                        logoArray.push("BLANK");
                    else
                        logoArray.push(new File(logoCommercialArray[0]));
                }
                if(logoCommercialArray[1] != "")
                {
                    if (logoCommercialArray[1] == "BLANK")
                        commercialsArray.push("BLANK");
                    else
                        commercialsArray.push(new File(logoCommercialArray[1]));
                }
            }
            spreadsheetFile.close();
        }
    }catch(e){
        alert("Error occrured at: decodeSpreadsheet");
        alert(e);
        throw Error("Unexpected error");
    }
}

//============================================================================

function convrtProjToFrameIO (commentProjFile)
/*
* Parameter: project file object 
*
* Algorithm:
*   The project is imported into the current project.
*   The comp inside the project is located
*   The first layer is identified as the text layer with comments in the "Source Text" property
*   The comments are transfered into a null layer
*   The null layer is assigned to global "nullMarkerLayer"
*   The comment text layer is deleted
*
* Conditions:
*   There should be only one comp in the imported project
*   The first layer in that comp should be the comment text layer
*/
{
    try
    {
        var compObj = null;
        var comTxtLayer = null;
         
        var cProjFolder = proj.importFile( new ImportOptions(commentProjFile) );
        
        compObj = findCommentCompRecurse (cProjFolder);

        if (compObj == null)
        {
            alert("No comps present in the project.");
            throw Error("Unexpected Error");
        }
    
        if (compObj.layer(2).hasVideo)
        {
            infoVidCenterX = compObj.layer(2).anchorPoint.value[0];
            infoVidCenterX *= (compObj.layer(2).scale.value[0]/100);           
        }else{
            alert ("The second layer in the comp should be the video layer to get the center position");
        }
    
        if (compObj.layer(1).property("Source Text") == null)
        {
            alert("First layer in comp should be the comment text layer.");
            throw Error ("Unexpected Error");
        }
    
        comTxtLayer = compObj.layer(1);
        nullMarkerLayer = compObj.layers.addNull (comTxtLayer.outPoint);
        
        for (var j=1; j<=comTxtLayer.property("Source Text").numKeys; j++)
        {
            var curKeyTxt = comTxtLayer.property("Source Text").keyValue(j).text;
            if (curKeyTxt != "")
            {
                var firstPart = curKeyTxt.substring(0, curKeyTxt.indexOf(" - " ) ) +  ": ";
                
                // gets second ":"
                var secPart = curKeyTxt.substring( curKeyTxt.indexOf(":")+1);
                secPart = secPart.substring(secPart.indexOf(":")+2);
                
                var editedTxt = firstPart + secPart;
                
                var newMarker = new MarkerValue("Text Marker-" +  j);
                newMarker.comment = editedTxt;
                nullMarkerLayer.property("Marker").setValueAtTime (comTxtLayer.property("Source Text").keyTime(j), newMarker);
            }
        }
        comTxtLayer.remove();
      
        
    }catch(e){
        alert("Error occured at:convrtProjToFrameIO");
        alert(e);
        throw Error ("Uexpected error");
    }
}


function findCommentCompRecurse (foldItem)
/*
* Recursively searches the (comment) project folder to find the comp containing the coment layer.
*/
{
    var compItem = null; 
    for (var i=1; i<=foldItem.numItems; i++)
    {
        if (foldItem.item(i).typeName == "Composition")
            compItem = foldItem.item(i);
        else if (foldItem.item(i).typeName == "Folder")
        {
            var retrnItem = findCommentCompRecurse (foldItem.item(i));
            if (retrnItem != null)
                compItem = retrnItem;
        }
    }
    return compItem;
}
//============================================================================

function createVideo(finalVidName)
/*
* Handles the creation of one video (AE project) using the given parameters.
*/
{
    controlPanelGUI.update();
    try{
        findComInfoVidTransComp ();
        var mainComp = proj.items.addComp(vidTitle, vidDimens[0], vidDimens[1], 1, 60, 30);
        var totTime = 0;
        if (curComVidFile == "BLANK")
        {
            totTime += addIntroVid (mainComp, true);  
            totTime += addComInfoTransition (mainComp, totTime);
            totTime += addInfoVid (mainComp, totTime);
            totTime += addOutro (mainComp, totTime); 
        }else{
            totTime += addIntroVid (mainComp, false);
            totTime += addComVid (mainComp, totTime);
            totTime += addComInfoTransition (mainComp, totTime);
            totTime += addInfoVid (mainComp, totTime);
            totTime += addOutro (mainComp, totTime);
        }
        mainComp.duration = totTime;
        var renderFold = proj.items.addFolder("Render-Folder");
        mainComp.parentFolder = renderFold;
        var finalComp = proj.items.addComp(finalVidName, finalVidDimens[0], finalVidDimens[1], 1, totTime, 30);
        finalComp.bgColor = videoBGColor;
        finalComp.layers.addSolid(videoBGColor, "Background", finalVidDimens[0], finalVidDimens[1], 1, totTime);
        var finalVideoLayer =  finalComp.layers.add(mainComp);
        finalVideoLayer.scale.setValue([(finalVidDimens[0]/vidDimens[0])*100, (finalVidDimens[1]/vidDimens[1])*100]);
    }catch(e){
        alert("Error occrured at: createVideo");
        alert(e);
        throw Error("Unexpected error");    
    }
}


function findComInfoVidTransComp ()
/*
* Finds the video introduction comp and assigns it to the "comInfoVidTransComp" variable.
*/
{
    controlPanelGUI.update();
    try{
        var i=1;
        for (i=1; i<=proj.numItems; i++)
        {
            if (proj.item(i).name == "Commercial-Info-Video-Transition")
                comInfoVidTransComp = proj.item(i); 
        }
    }catch(e){
        alert("Error occrured at: findComInfoVidTransComp");
        alert(e);
        throw Error("Unexpected error");    
    }
}


function addIntroVid (mainCompObj, noCommercial)
/*
* This function adds the intro video to the main composition. The duration
* of the intro video is returned.
*/
{
    controlPanelGUI.update();
    try{
        if (introFile.exists)
        {
            var introItem = proj.importFile(new ImportOptions(introFile));
            var introLayer = mainCompObj.layers.add(introItem);
            resizeComp (introLayer, vidDimens);
            if (!noCommercial)
            {
                introLayer.opacity.setValueAtTime(introItem.duration - introComIndentTime, 100);
                introLayer.opacity.setValueAtTime(introItem.duration, 0);
            }
            return introItem.duration;
         }
         alert("The intro file or file path is invalid. Please run the script again.");
         throw Error("Error.");
         return null;
    }catch(e){
        alert("Error occrured at: addIntroVid ");
        alert(e);
        throw Error("Unexpected error");
    }
}
    
function addComVid (mainCompObj, curTime)
/*
* This function adds the commercial to the main composition. The duration of the commercial 
* (minus the intersection duration) is returned.
*/
{
    controlPanelGUI.update();
    try{
        if (curComVidFile.exists)
        {
            var comItem = proj.importFile(new ImportOptions(curComVidFile));
            var comVidLayer = mainCompObj.layers.add(comItem);
            comVidLayer.startTime = curTime - introComIndentTime;
            comVidLayer.moveToEnd();
            resizeComp (comVidLayer, vidDimens);
            return comItem.duration - introComIndentTime;
        }
        alert("A commercial file doesn't exist. Make sure all commercial file paths are correct.");
        alert("File name: " + curComVidFile.name);
        throw Error("Error.");
        
    }catch(e){
        alert("Error occrured at: addComVid");
        alert(e);
        throw Error("Unexpected error");
    }
}

function addComInfoTransition (mainCompObj, curTime)
/*
* This function sets the video title and description in the video introduction comp. It then adds the 
* video introduction comp to the main composition. The duration of the video introduction 
* comp (minus the intersection duration) is returned.
*/
{
    controlPanelGUI.update();
    try{
        var titleAndDescComp = null;
        var i = 1;
        for (i=1; i<=proj.numItems; i++)
        {
            if (proj.item(i).name == "Transition-SubTitle-And-Description")
                titleAndDescComp = proj.item(i);
        }
        var comInforTransLayer = mainCompObj.layers.add(comInfoVidTransComp);
        titleAndDescComp.layer(1).property("Source Text").setValue(vidTitle);
        titleAndDescComp.layer(2).property("Source Text").setValue(vidDescription);
        
        // Corrects subtitle (video description) layer position if title is large
        var transTitleHeight = titleAndDescComp.layer(1).sourceRectAtTime(0, false).height;
        var defaultTitleHeight = 64;
        if (transTitleHeight > defaultTitleHeight)
        {
            var curDesPos = titleAndDescComp.layer(2).position.value;
            titleAndDescComp.layer(2).position.setValue([curDesPos[0], curDesPos[1] + (transTitleHeight-defaultTitleHeight)]);
        }    
        
        comInforTransLayer.startTime = curTime - 2.5;
        resizeComp (comInforTransLayer, vidDimens);
        return comInfoVidTransComp.duration - 2.5;
    }catch(e){
        alert("Error occrured at: addComInfoTransition");
        alert(e);
        throw Error("Unexpected error")
    }
}

function addInfoVid (mainCompObj, curTime)
/*
* This function adds the information video to the main composition. If specified, it also superimposes the 
* selected logo on top of the information video. The duration of the information video (minus the intersection duration) is returned.
*/ 
{
    controlPanelGUI.update();
    try{
        if (curInfoVidFile.exists)
        {
            infoVidItem = proj.importFile(new ImportOptions(curInfoVidFile));
            infoVidLayer = mainCompObj.layers.add(infoVidItem);
            resizeComp (infoVidLayer, vidDimens);
            infoVidLayer.startTime = curTime - transInfoVidIndentTime;
            infoVidLayer.moveAfter(mainCompObj.layer(2));
            
            if (curLogoFile != "BLANK")
                addInfoVidLogo (mainCompObj);
                
            createHalfScreenAnim (mainCompObj);
            createNameTagAnim (mainCompObj);
            return infoVidItem.duration - transInfoVidIndentTime;
        }
        alert("A Main video doesn't exist or is invalid. Please run the script again.");
        alert("File name: " + curInfoVidFile.name);
        throw Error("Error.");
        return null;
    }catch(e){
        alert("Error occrured at: addInfoVid");
        alert(e);
        throw Error("Unexpected error");
    }
}


function addOutro (mainCompObj, curTime)
/*
* This function adds the outro video to the main composition. The duration of the outro video 
* (minus the intersection duration) is returned.
*/
{
    controlPanelGUI.update();
    try{
        if (outroFile.exists)
        {
            var outroItem = proj.importFile(new ImportOptions(outroFile));
            var outroLayer = mainCompObj.layers.add(outroItem);
            outroLayer.startTime = curTime - infoVidOutroIndentTime;
            outroLayer.opacity.setValueAtTime(outroLayer.startTime, 0);
            outroLayer.opacity.setValueAtTime(outroLayer.startTime + infoVidOutroIndentTime, 100);
            resizeComp (outroLayer, vidDimens);
            return outroItem.duration - infoVidOutroIndentTime;
        }
         alert("The outro file or file path is invalid. Please run the script again.");
         throw Error("Error.");
         return null;
    }catch(e){
        alert("Error occrured at: addOutro");
        alert(e);
        throw Error("Unexpected error");
    }
}

function addInfoVidLogo (mainCompObj)
/*
* This function superimposes the logo on top of the information video layer.
*/
{
    controlPanelGUI.update();
    try{
        if (curLogoFile.exists)
        {
            var logoItem = proj.importFile(new ImportOptions(curLogoFile));
            logoLayer = mainCompObj.layers.add(logoItem);
            logoLayer.moveBefore(infoVidLayer);
            resizeComp (logoLayer, logoDimens);
            logoLayer.position.setValue([vidDimens[0]*0.83, vidDimens[1]*0.9]);
            logoLayer.startTime = infoVidLayer.startTime - (2/30);
            logoLayer.outPoint = infoVidItem.duration + logoLayer.startTime;
        }else{
            alert("A logo file or file path is invalid. Please run the script again.");
            alert("File name: " + curLogoFile.name);
            throw Error("Error.");
        }
    }catch(e){
        alert("Error occrured at: addInfoVidLogo");
        alert(e);
        throw Error("Unexpected error");
    }
}
    
    
function resizeComp (layerObj, resizeDimens)
/*
* This function resizes the passed-in layer (layerObj) such that the whole layer fits inside the
* passed-in dimensions (resizeDimens).
*/
{
    controlPanelGUI.update();
    try{
        var widthRatio = resizeDimens[0] / layerObj.width;
        var heightRatio = resizeDimens[1] / layerObj.height;
        
        if (widthRatio < heightRatio)
            layerObj.scale.setValue([widthRatio*100,widthRatio*100]);
        else
            layerObj.scale.setValue([heightRatio*100,heightRatio*100]);
    }catch(e){
        alert("Error occrured at: resizeComp");
        alert(e);
        throw Error("Unexpected error");
    }
}

function createNameTagAnim (mainCompObj)
/*
* This function handles the creation of the dr. name and location tag animation.
* It changes the dr. name and location to the user defined values and then animates the
* tags.
*/
{
    controlPanelGUI.update();
    try{
        var drNameLayerYPos = (320/360) * vidDimens[1];
        //outAnimStrtTime = nullMarkerLayer.marker.keyTime(5) + infoVidLayer.startTime;
        drNameAndProf = nullMarkerLayer.marker.keyValue(3).comment.substring(nullMarkerLayer.marker.keyValue(3).comment.indexOf(": ")+2);
        drLocation = nullMarkerLayer.marker.keyValue(4).comment.substring(nullMarkerLayer.marker.keyValue(4).comment.indexOf(": ")+2);
        
        var drNameComp = null;
        var j=1;
        for (j=1; j<=proj.numItems; j++)
        {
            if (proj.item(j).name == "Name-Tag")
                drNameComp = proj.item(j);
        }

        drNameComp.layer(2).property("Source Text").setValue(drNameAndProf);
        drNameComp.layer(1).property("Source Text").setValue(drLocation);
        
        drNameWidth = drNameComp.layer(2).sourceRectAtTime(0, false).width;
        drLocWidth = drNameComp.layer(1).sourceRectAtTime(0, false).width;

        var origDrNameCompWidth = drNameComp.width;
        var maxTextWidth = 0;
        if (drNameWidth > drLocWidth)
            maxTextWidth = drNameWidth;
        else
            maxTextWidth = drLocWidth;

        drNameComp.width = Math.round(maxTextWidth * 1.1);

        drNameComp.layer(1).position.setValue([Math.round(drNameComp.width/2), drNameComp.layer(1).position.value[1]]);   
        drNameComp.layer(2).position.setValue([Math.round(drNameComp.width/2), drNameComp.layer(2).position.value[1]]);   

        drNameLayer = mainCompObj.layers.add(drNameComp);
        drNameLayer.moveBefore(infoVidLayer);
        
        var inAnimStrtTime = infoVidLayer.startTime + 0.3 + transInfoVidIndentTime;
        // Animation of whole "Name-Tag" comp
        drNameLayer.position.setValueAtTime(inAnimStrtTime, [-(drNameComp.width/2)-2, drNameLayerYPos]);
        drNameLayer.position.setValueAtTime(inAnimStrtTime + 1, [drNameComp.width/2, drNameLayerYPos]);
        drNameLayer.position.setValueAtTime(inAnimStrtTime + 6, [drNameComp.width/2, drNameLayerYPos]);
        drNameLayer.position.setValueAtTime(inAnimStrtTime + 7, [-(drNameComp.width/2)-2, drNameLayerYPos]);

        var k = 1;
        for (k=1; k<=drNameLayer.position.numKeys; k++)
            drNameLayer.position.setInterpolationTypeAtKey(k, KeyframeInterpolationType.BEZIER);  

        // Animation of textboxes in "Name-Tag" comp
        var origDrNamePos = drNameComp.layer(2).position.value;
        drNameComp.layer(2).position.setValueAtTime(inAnimStrtTime, origDrNamePos - (drNameWidth*1.2));
        drNameComp.layer(2).position.setValueAtTime(inAnimStrtTime + 1.3, origDrNamePos);
        drNameComp.layer(2).position.setInterpolationTypeAtKey(1, KeyframeInterpolationType.BEZIER);  
        drNameComp.layer(2).position.setInterpolationTypeAtKey(2, KeyframeInterpolationType.BEZIER); 
        
        var origDrLocPos = drNameComp.layer(1).position.value;
        drNameComp.layer(1).position.setValueAtTime(inAnimStrtTime, origDrLocPos - (drLocWidth*1.2));
        drNameComp.layer(1).position.setValueAtTime(inAnimStrtTime + 2, origDrLocPos);
        drNameComp.layer(1).position.setInterpolationTypeAtKey(1, KeyframeInterpolationType.BEZIER);  
        drNameComp.layer(1).position.setInterpolationTypeAtKey(2, KeyframeInterpolationType.BEZIER);
    }catch(e){
        alert("Error occrured at: createNameTagAnim");
        alert(e);
        throw Error("Unexpected error");
    }
}
    
/*
function createHalfScreenAnim (mainCompObj)
{
    controlPanelGUI.update();
    try{
        var halfScreenComp = proj.items.addComp("Half-Screen", vidDimens[0], vidDimens[1], 1, 600, 30);
        var halfScreenFold = proj.items.addFolder("Half-Screen-Folder");
        halfScreenComp.parentFolder = halfScreenFold;
        halfScreenComp.duration = nullMarkerLayer.marker.keyTime(nullMarkerLayer.marker.numKeys) + 20;
        var whiteBG = halfScreenComp.layers.addSolid([1,1,1], "White-BG", vidDimens[0], vidDimens[1], 1, halfScreenComp.duration); 
        var halfScreenLayer = mainCompObj.layers.add(halfScreenComp);
        halfScreenLayer.moveBefore(infoVidLayer);
        halfScreenLayer.startTime = infoVidLayer.startTime;
        
       halfScreenOutPos = [halfScreenLayer.position.value[0] + halfScreenLayer.width, halfScreenLayer.position.value[1]];
       halfScreenInPos = [halfScreenOutPos[0] - (halfScreenLayer.width/2), halfScreenLayer.position.value[1]];
       
        var lastUsedMarker = 4
        while (lastUsedMarker != nullMarkerLayer.marker.numKeys)
        {
            var subtitleReturn = createSubtitles (halfScreenComp, lastUsedMarker+1);

            var halfAnimStrtTime = nullMarkerLayer.marker.keyTime(lastUsedMarker+1) + infoVidLayer.startTime;
            var halfAnimFinTime = subtitleReturn[0] + infoVidLayer.startTime + halfScreenPaddingTime;
            
            lastUsedMarker = subtitleReturn[1];

            halfScreenLayer.position.setValueAtTime(halfAnimStrtTime, halfScreenOutPos);
            halfScreenLayer.position.setValueAtTime(halfAnimStrtTime + 2.0, halfScreenInPos);
            halfScreenLayer.position.setValueAtTime(halfAnimFinTime, halfScreenInPos);
            halfScreenLayer.position.setValueAtTime(halfAnimFinTime + 2.0, halfScreenOutPos);
      
            var k = 1;
            for (k=1; k<=halfScreenLayer.position.numKeys; k++)
                halfScreenLayer.position.setInterpolationTypeAtKey(k, KeyframeInterpolationType.BEZIER);
            
            var origVidPos = infoVidLayer.position.value;
            var infoVidHalfXPos = infoVidCenterX - (vidDimens[0]/4);
            if (infoVidHalfXPos<0)
                infoVidHalfXPos = origVidPos[0];
            else
                infoVidHalfXPos = origVidPos[0] - infoVidHalfXPos;
           

           infoVidLayer.position.setValueAtTime(halfAnimStrtTime, origVidPos);
           infoVidLayer.position.setValueAtTime(halfAnimStrtTime + 2.0, [infoVidHalfXPos, origVidPos[1]]);
           infoVidLayer.position.setValueAtTime(halfAnimFinTime, [infoVidHalfXPos, origVidPos[1]]);
           infoVidLayer.position.setValueAtTime(halfAnimFinTime + 2.0, origVidPos);

            k = 1;
            for (k=1; k<=infoVidLayer.position.numKeys; k++)
                infoVidLayer.position.setInterpolationTypeAtKey(k, KeyframeInterpolationType.BEZIER);
        }
    }catch(e){
        alert("Error occrured at: createHalfScreenAnim");
        alert(e);
        throw Error("Unexpected error");
    }
}
*/


function createHalfScreenAnim (mainCompObj)
{
    controlPanelGUI.update();
    try{
        var topicInteval = 20; // if the number of seconds between topics is less than this value, the half-screen stays
        var halfScreenComp = proj.items.addComp("Half-Screen", vidDimens[0], vidDimens[1], 1, 600, 30);
        var halfScreenFold = proj.items.addFolder("Half-Screen-Folder");
        halfScreenComp.parentFolder = halfScreenFold;
        halfScreenComp.duration = nullMarkerLayer.marker.keyTime(nullMarkerLayer.marker.numKeys) + 20;
        var whiteBG = halfScreenComp.layers.addSolid([1,1,1], "White-BG", vidDimens[0], vidDimens[1], 1, halfScreenComp.duration); 
        var halfScreenLayer = mainCompObj.layers.add(halfScreenComp);
        halfScreenLayer.moveBefore(infoVidLayer);
        halfScreenLayer.startTime = infoVidLayer.startTime;
        
       halfScreenOutPos = [halfScreenLayer.position.value[0] + halfScreenLayer.width, halfScreenLayer.position.value[1]];
       halfScreenInPos = [halfScreenOutPos[0] - (halfScreenLayer.width/2), halfScreenLayer.position.value[1]];
       
        var curMarkerIndx = 4;
        while (curMarkerIndx != nullMarkerLayer.marker.numKeys)
        {
            var halfScreenAnimIn = true, 
                    halfScreenAnimOut = true;
            
            // checks if the time difference between the previous topic and the current topic is less than "topicInterval" 
            // time difference: (new-topic-heading) - (last-topic-last-point)
            if (curMarkerIndx > 4)
            {
                if (nullMarkerLayer.marker.keyTime(curMarkerIndx+1) - nullMarkerLayer.marker.keyTime(curMarkerIndx) < topicInteval)
                    halfScreenAnimIn = false; // Do not animate-in because previous half-screen would be remaining
            }
                                        
            var subtitleReturn = createSubtitles (halfScreenComp, curMarkerIndx+1);
            var halfAnimStrtTime = nullMarkerLayer.marker.keyTime(curMarkerIndx+1) + infoVidLayer.startTime;
            var halfAnimFinTime = subtitleReturn[0] + infoVidLayer.startTime + halfScreenPaddingTime;
            
            curMarkerIndx = subtitleReturn[1];
            
            // checks if the time difference between the current topic and the next topic is less than "topicInterval"
            // time difference: (current-topic-last-point) - (next-topic-heading)
            if (curMarkerIndx != nullMarkerLayer.marker.numKeys)
            {
                if (nullMarkerLayer.marker.keyTime(curMarkerIndx+1) - nullMarkerLayer.marker.keyTime(curMarkerIndx) < topicInteval)
                    halfScreenAnimOut = false; // Do not animate-out because next half-screen will not animate-in
            }

            var origVidPos = infoVidLayer.position.value;
            var infoVidHalfXPos = infoVidCenterX - (vidDimens[0]/4);
            if (infoVidHalfXPos<0)
                infoVidHalfXPos = origVidPos[0];
            else
                infoVidHalfXPos = origVidPos[0] - infoVidHalfXPos;
                
            // If half-screen should be animated-in
            //var halfAnimStrtTime = nullMarkerLayer.marker.keyTime(curMarkerIndx+1) + infoVidLayer.startTime;
            if (halfScreenAnimIn)
            {
                halfScreenLayer.position.setValueAtTime(halfAnimStrtTime, halfScreenOutPos);
                halfScreenLayer.position.setValueAtTime(halfAnimStrtTime + 2.0, halfScreenInPos);   
                
               infoVidLayer.position.setValueAtTime(halfAnimStrtTime, origVidPos);
               infoVidLayer.position.setValueAtTime(halfAnimStrtTime + 2.0, [infoVidHalfXPos, origVidPos[1]]);
            }
        
            // If half-screen should be animated-out
            if (halfScreenAnimOut)
            {     
                halfScreenLayer.position.setValueAtTime(halfAnimFinTime, halfScreenInPos);
                halfScreenLayer.position.setValueAtTime(halfAnimFinTime + 2.0, halfScreenOutPos);     
                
               infoVidLayer.position.setValueAtTime(halfAnimFinTime, [infoVidHalfXPos, origVidPos[1]]);
               infoVidLayer.position.setValueAtTime(halfAnimFinTime + 2.0, origVidPos);
            }
        }
    
        var k = 1;
        for (k=1; k<=halfScreenLayer.position.numKeys; k++)
            halfScreenLayer.position.setInterpolationTypeAtKey(k, KeyframeInterpolationType.BEZIER);
            
        k = 1;
        for (k=1; k<=infoVidLayer.position.numKeys; k++)
            infoVidLayer.position.setInterpolationTypeAtKey(k, KeyframeInterpolationType.BEZIER);
            
    }catch(e){
        alert("Error occrured at: createHalfScreenAnim");
        alert(e);
        throw Error("Unexpected error");
    }
}


function createSubtitles (halfScreenComp, strtMarkerIndex)
{
    controlPanelGUI.update();
    try{
        var subtitleTxtBoxDimens = [vidDimens[0]*0.42, vidDimens[0]* 0.2];
        var subtitleTxtBoxPos = [vidDimens[0]/4, (0.08*vidDimens[1])+(subtitleTxtBoxDimens[1]/2)]; // Starting position of the subtitle textbox
        var subtitleTitleFont = [subsHeadingFont, subsHeadingFontSize]; // Array representing the font and font size for a subtitle Title.
        var subtitlePointFont = [subsPointFont, subsPointFontSize]; // Array representing the font and font size for a subtitle Point.
        var subtitleSubPointFont = [subsSubPointFont, subsSubPointFontSize]; // Array representing the font and font size for a subtitle Subpoint.
        var subtitleSubPointXPos = subtitleTxtBoxPos[0] + ((17/640)*vidDimens[0]); // Repositioning X-coordinate for subtitle subpoint 
        var writingSpeed = 75/38; // Number-of-frames / Number-of-characters. Used to estimate writing (animating) speed.
        var titleHeight = ((56/360) * vidDimens[1]) * 0.8; // Filler height if the previous subtitle is: title 
        var subtitlePointHeight = ((44/360) * vidDimens[1]) * 0.6; // Filler height if the previous subtitle is: Point
        var subtitlePointSubPointHeight = (12/360) * vidDimens[1]; // Filler height if the previous subtitle is: Point. And the current subtitle is: Subpoint.
        var subtitleSubpointSubpointHeight = (6/360) * vidDimens[1]; // Filler height if the previous subtitle is: Subpoint. And the current subtitle is: Subpoint.
        var maxMsgHeight = (300/360) * vidDimens[1]; // The maximum height that can be occupied by text. If there is more text, the current text is erased and the new text is written from the beginning.
        
        var halfDimens = [vidDimens[0]/2, vidDimens[1]/2];
        var curTextPos = subtitleTxtBoxPos; // Starting position (x,y, coords)
        var subtitleLayerArray = []; // Holds all the subtitle layers.
        var prevSubtitleType = null; // The previous subtitle type. Data type: String. Possible types are: null, "title", "point", "subpoint"
        var afterTitleYPos = 0; // The Y position after the title.
        var finalUsedMarker = nullMarkerLayer.marker.numKeys // The final marker index of the current half-screen
        var titleLayer = null; // The current title layer.
        var i = strtMarkerIndex;
        for (i=strtMarkerIndex; i<=nullMarkerLayer.marker.numKeys; i++)
        {
            var textStr = nullMarkerLayer.marker.keyValue(i).comment.substring(nullMarkerLayer.marker.keyValue(i).comment.indexOf(": ")+2);
            var subtitleLayer = halfScreenComp.layers.addBoxText(subtitleTxtBoxDimens);
            subtitleLayer.position.setValue(curTextPos);
            
            // If marker is a Title
            if (nullMarkerLayer.marker.keyValue(i).comment.indexOf("*") == -1)
            {
                if (titleLayer != null)
                {
                    halfScreenComp.layer(1).remove();
                    finalUsedMarker = i-1;
                    break;
                }
                else
                {
                    titleLayer = subtitleLayer;
                }
                titleLayer.opacity.setValueAtTime(nullMarkerLayer.marker.keyTime(i)-0.5, 0);
                titleLayer.opacity.setValueAtTime(nullMarkerLayer.marker.keyTime(i), 100);
                
                subtitleLayer.property("Source Text").setValue(textStr);
                applySubtitleFormating (subtitleLayer, subtitleTitleFont, true);
                var textRect = subtitleLayer.sourceRectAtTime(0, false);
                curTextPos = [subtitleTxtBoxPos[0], subtitleLayer.position.value[1] + textRect.top + textRect.height];
                afterTitleYPos = subtitleLayer.position.value[1] + textRect.top + textRect.height;
                prevSubtitleType = "title";
            }
            // If marker is a Point
            else if (nullMarkerLayer.marker.keyValue(i).comment.indexOf("**") == -1)
            {
                textStr = trimWhitespaces(textStr.substring(1));
                subtitleLayer.property("Source Text").setValue(textStr);
                applySubtitleFormating (subtitleLayer, subtitlePointFont, false);
                
                var textRect = subtitleLayer.sourceRectAtTime(0, false);
                
                var heightChange = 0;
                if (prevSubtitleType == "title")
                    heightChange = titleHeight;
                else
                    heightChange = subtitlePointHeight;

                if ((curTextPos[1] + heightChange + textRect.height) > maxMsgHeight)
                {
                    prevSubtitleType = restartWritingFromTop (i, subtitleLayerArray);
                    curTextPos[1] = afterTitleYPos;
                    heightChange = titleHeight;
                }
                subtitleLayer.position.setValue([subtitleLayer.position.value[0], curTextPos[1] + heightChange - textRect.top]);
                
                bulletTextbox = halfScreenComp.layers.addBoxText(subtitleTxtBoxDimens);
                bulletTextbox.property("Source Text").setValue(String.fromCharCode(8226));
                bulletTextbox.moveAfter(subtitleLayer);
                applySubtitleFormating (bulletTextbox, subtitlePointFont, false);
                bulletRect = bulletTextbox.sourceRectAtTime(0, false);
                bulletTextbox.position.setValue([subtitleLayer.position.value[0]-(bulletRect.width*2) + overridePointBulltetPointVal, subtitleLayer.position.value[1]]);
                 
                curTextPos = [subtitleTxtBoxPos[0], subtitleLayer.position.value[1] + textRect.top + textRect.height];
                    
                applyAnimation (textStr.length, subtitleLayer, nullMarkerLayer.marker.keyTime(i), writingSpeed, bulletTextbox);   
                subtitleLayerArray.push(subtitleLayer);
                subtitleLayerArray.push(bulletTextbox);
                prevSubtitleType = "point";
            }
            // If marker is a Sub-point
            else if (nullMarkerLayer.marker.keyValue(i).comment.indexOf("**") != -1)
            {
                textStr = trimWhitespaces(textStr.substring(2));
                subtitleLayer.property("Source Text").setValue(textStr);
                applySubtitleFormating (subtitleLayer, subtitleSubPointFont, false);
                
                var textRect = subtitleLayer.sourceRectAtTime(0, false);
                
                var heightChange = 0;
                if (prevSubtitleType == "point")
                    heightChange = subtitlePointSubPointHeight;
                else
                    heightChange = subtitleSubpointSubpointHeight;

                if ((curTextPos[1] + heightChange + textRect.height) > maxMsgHeight)
                {
                    prevSubtitleType = restartWritingFromTop (i, subtitleLayerArray);
                    curTextPos[1] = afterTitleYPos;
                    heightChange = titleHeight;
                }
                subtitleLayer.position.setValue([subtitleSubPointXPos, curTextPos[1] + heightChange - textRect.top]);

                bulletTextbox = halfScreenComp.layers.addBoxText(subtitleTxtBoxDimens);
                bulletTextbox.property("Source Text").setValue(String.fromCharCode(45));
                bulletTextbox.moveAfter(subtitleLayer);
                applySubtitleFormating (bulletTextbox, subtitleSubPointFont, false);
                bulletRect = bulletTextbox.sourceRectAtTime(0, false);
                bulletTextbox.position.setValue([subtitleLayer.position.value[0]-(bulletRect.width*2), subtitleLayer.position.value[1]]);
                curTextPos = [subtitleTxtBoxPos[0], subtitleLayer.position.value[1] + textRect.top + textRect.height];
                    
                applyAnimation (textStr.length, subtitleLayer, nullMarkerLayer.marker.keyTime(i), writingSpeed, bulletTextbox);
                subtitleLayerArray.push(subtitleLayer);
                subtitleLayerArray.push(bulletTextbox);
                prevSubtitleType = "subpoint";
            }  
        }

        var finalTime = halfScreenComp.layer(1).Text.Animators.property("ADBE Text Animator")("ADBE Text Selectors")("ADBE Text Selector")("End").keyTime(2);
        
        k=1;
        for (k=1; k<=halfScreenComp.numLayers-1; k++)
        {
            var curLayerOpacity = halfScreenComp.layer(k).opacity;
            curLayerOpacity.setValueAtTime(finalTime + halfScreenPaddingTime + 3, curLayerOpacity.valueAtTime(finalTime + halfScreenPaddingTime, false));
            curLayerOpacity.setValueAtTime(finalTime + halfScreenPaddingTime + 4, 0);
        }

        return [finalTime, finalUsedMarker];
    }catch(e){
        alert("Error occrured at: createSubtitles");
        alert(e);
        throw Error("Unexpected error");
    }
}

function restartWritingFromTop (curMarkerIndex, subtitleLayerArray)
{
    try{
        var midTime = nullMarkerLayer.marker.keyTime(curMarkerIndex) - 1/2;
        var j =0;
        for (j=0; j<subtitleLayerArray.length; j++)
        {
            subtitleLayerArray[j].opacity.setValueAtTime(midTime, subtitleLayerArray[j].opacity.valueAtTime(midTime,false));
            subtitleLayerArray[j].opacity.setValueAtTime(midTime+(1/3), 0);
        }
        return "title";
    }catch(e){
        alert("Error occrured at: restartWritingFromTop");
        alert(e);
        throw Error("Unexpected error");
    }
}

function applySubtitleFormating (txtLayer, subsFontArray, subJustify)
{   
    try{
        var subtitleDoc = txtLayer.property("Source Text").value;
        subtitleDoc.font = subsFontArray[0];
        subtitleDoc.fontSize = subsFontArray[1];
        subtitleDoc.fillColor = [0.05, 0.05, 0.05];
        subtitleDoc.applyStroke = false;
        subtitleDoc.applyFill = true;
        
        if (subJustify)
        {
            subtitleDoc.justification = ParagraphJustification.CENTER_JUSTIFY;
        }
         else
        {
            subtitleDoc.justification = ParagraphJustification.LEFT_JUSTIFY;
        }

        txtLayer.property("Source Text").setValue(subtitleDoc);
    }catch(e){
        alert("Error occrured at: applySubtitleFormating");
        alert(e);
        throw Error("Unexpected error");  
    }
}

function applyAnimation (strLength, txtLayer, animStrtTime, framesPerChar, bulletLayer)
/*
* This function handles the in-animation of the "txtLayer" parameter. It creates the pseudo-typing effect.
*/
{
    try{
        var animEndTime = animStrtTime + ((framesPerChar*strLength)/30);
        var textAnim = txtLayer.Text.Animators.addProperty("ADBE Text Animator");
        var rangeSel = textAnim.property("ADBE Text Selectors").addProperty("ADBE Text Selector");
        var animOpa = textAnim("ADBE Text Animator Properties").addProperty("ADBE Text Opacity");
        animOpa.setValue(0);
        rangeSel("Start").setValue(100);
        rangeSel("Offset").setValue(0);
        rangeSel("End").setValueAtTime(animStrtTime, 0);
        rangeSel("End").setValueAtTime(animEndTime, 100);
        bulletLayer.opacity.setValueAtTime(animStrtTime-0.3, 0);
        bulletLayer.opacity.setValueAtTime(animStrtTime, 100);
    }catch(e){
        alert("Error occrured at: applyAnimation");
        alert(e);
        throw Error("Unexpected error");  
    }
}
   
function trimWhitespaces(x)
/*
* This function removes any leading and trailing whitespaces of the passed-in string
*/
{
    return x.replace(/^\s+|\s+$/gm,'');
}
