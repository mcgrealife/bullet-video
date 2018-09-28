# Overview
An Adobe After Effects script that takes timestamped text comments from video review applications such as frame.io or wipster.io, and transforms them into animated bullet points. It's great for educational lecture videos, to visually anchor the speakers thoughts.

In addition to beautiful bullet-text animations, it is designed to batch replace commercials and logos. This is useful for educational websites which change sponsors frequently. So you can easily swap commercial and logos for the "sponsor of the month", and then update video instances, while preserving your analytics and URL (especially if using business video hosting service such as Wistia)

# FAQ
- This should be saved as a .jsx file, and stored in the Adobe After Effects Script folder.
  - If windows, ensure your script folder has write access (so it can save the settings file, which saves your previous file pathways in the script gui)
- source video should have the exact same name as the .aep
- source video and .aep should be in the same folder
- spreadsheet should have two columns. First column = local "commercial" pathway; second column = logo pathway
  - we recommend a pure white logo
  - optionally type "BLANK" in each cell to skip.
  - can have as many rows as you'd like. Each row = an instance of the video (with varying commercials / logos), for .aep file located in the main folder at the time of script run. 
- need 4 seconds between previous bullet or sub-bullet and new title, otherwise there will be text overlap. If the next title appears within 4-6 seconds after the previous bullet or sub-bullet, the halfscreen will stay but the previous text will fade.
- After running script, a new .aep file will be output. In can be useful to render these to an Adobe Media Encoder watch folder, for auto rendering. If there is a matching .aep file in the watch folder, it will be overwritten.
- Must include `video-creator-template.aep` in the same scripts folder. Change the template, or update line 32, and save as new script, to have multiple script versions which run on different templates. This may be a useful future variable to offer on the script gui.
  - if the script encounters errors, it will usually end with the video-creator-template.aep open-- be careful not to click "save", otherwise you will need to download a fresh copy of the template.
- text charcacter settings other than "font size" and "type weight" will be taken from your current settings. Particularly the "all caps" "superscript" row of buttons, and "line height". A recommended line height is 55.
- After importing from frame.io, if you need to preview/edit text in AE, sometimes you have to expand the text guide layer, and click on the actual source text square keyframe for it to appear in the composition viewer.
   - if your bullets have "*" artifacts leftover, it probably means there is a space or two in front of the asterisk in frame.io and your source text parameter
