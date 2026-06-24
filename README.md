# Synaptic-Density-Analysis
For quantifying puncta density of a synaptic marker

This analysis is used to quantify the puncta density of a target of interest within an image. For synaptic densities, a chosen synaptic marker (VGLUT1, Bassoon, Homer) is quantified along a dendritic segment. This is for analysis of a single channel, not colocalization of two or more.

**1. Arrange channels + stack to .tif from .ims**
This macro allows for initial processing of raw image files.
  - Starting image folder selection (no nesting, metadata files are allowed)
  - Output folder selection
  - Initial prompt allows to:
     - Identify the number of channels within the image
      - How many channels you would like in the final image
      - The file extension type (to isolate/limit file list to that type)
      - If a projection is needed (blank does not perform a projection)
   - Next prompt allows for:
       - Identification of signal in each channel (additional channels can be customized within the macro)
       - Reorganization of channels (if starting vs ending channels differs, it will by default list in chronological order)
   - Files will then open individually and process. NOTE: depending on the stack selection, a suffix will be added to the new images (MAX, MIN, AVG)
   - Excel file listing starting/ending channel information will also be saved in output

**2. ROI Selection** 
This macro will open images and pompt for selection of an ROI (dendritic segment)
  - Starting image folder
  - Output folder for ROIs
  - Experiment Info Prompt
     - Experiment Title: To name/identify saved files
     - Pixel scale: to assign the correct scale
  - Channel Info Prompt
     - ROI Type: polygon is default, though freehand is particularly useful with a tablet or drawing pad
     - Number of channels within the starting images
     - Number of channels for final image (this is for a merged image. All channels will be extracted and saved as single channel files!)
  - Assigning Channel Colors: this allows for organization/identification of the single channel images saved
    - Channel color: this reorders the starting channels for your merged file as RGBC (2 channel file will select and order the channels labeled Red and Green, respectively) Make sure to select the appropriate color for each channel (though this can also be done manually after, using the single channel images)
  - Visualize in composite view: this allows you to set/limit which channels you are able to see when selecting an ROI
- Macro will then randomize and blind all image files for processing
- Selecting ROIS:
  - Macro will open image file and prompt if you have a new ROI in frame
  - Once selected, "OK" will add to the ROI manager and save single channel and merge files of that ROI
  - Boolean will continue to prompt for new ROIs. "No" will save all ROIs, exit file, and continue on
- Output/results will include:
  - Excel file with channel info
  - Folders containing single channel and merged image files of the ROIs

**3. ROI Length MinMax** 
This macro allows for standardization of ROI lengths within a chosen range. Anything under the minimum will be excluded, while those over will be prompted to make a smaller selection
-Starting images: choose a folder with images that you plan to quantify (this can be multichannel, though easier workflow by using the single channel images you plan to perform analysis)
- Output: folder for finalized pool of images for analysis, excel with length values
- Experiment Info Prompt:
  - Title: To properly name/identify result files
  - Scale: to set and calculate proper scale
  - Min Length: minimum acceptable length for ROI (scale value)
  - Max Length: max acceptable length for ROI (scale value)
 - Images will then open and you will be asked to draw the length of the roi (default is freehand, can be adjusted within the macro)
  - Acceptable length within the min/max and will be saved, length recorded
  - Lengths smaller than the minimum will close and not be recorded/saved
  - Lengths larger than the maximum allowed value will get an exception asking for a new length within the ROI. Once selected, if within the range a new image will be exctracted based on the new ROI length bounds. New image and length will be saved and recorded
- Output will include pool of ROI images within the set range, and an excel file with the measured lengths of each (scale values)
    
**4. Thresholding Single Channel** 
   This macro allows to record/set thresholding values in single channel images. 
   NOTE: for puncta analysis, only individual thresholding values are needed (no ROIs or masks needed).As this is for quantifying number of represetative puncta, individual thresholding is applicable. for signal intensity-based analysis and quantification, set thresholding is needed.
   - Starting image folder selection (images only, no nested folders)
   - Output folder selection (additional nested folders will automatically be generated based on selections)
   - Initial prompt:
       - Title: To properly name/identify result files
       - Record Threshold (process all files to get individual values)
       - Image type: assists with setting thresholding values
   - Record Thresholding Channel Customizations:
       - Auto Thresholding will record automated thresholding value. Uncheck for manual selection. NOTE: files are automatically blinded to reduce bias
       - Save Mask (not needed)
       - Save ROI (not needed)
   - Images will open sequentially and you will be prompted to select an appropriate threshold (unless auto is selected). Approrpiate thresholding for puncta analysis successfully isolates identifiable puncta from negative background (as if quantification was done manually by eye)
   - Results in Output folder will depend on selections:
       - Excel file containing randomized list of images
       - Excel file with individual thresholding values
          
**5. Single Channel Puncta Analysis** 
   This macro then takes selected single channel ROI images and performs maxima detection and particle analysis using the recorded thresholding value. NOTE: This is to quantify puncta, not examine signal intensity
   - Starting images: Single channel images of the target of interest to quantify
   - Output: Folder for results
   - Parameters:
     - Title: Experiment Title
     - Pixel scale: convert to appropriate scaling
     - Min Puncta Size: Lower limit to remove any small, possibly nonspecific particles
     - Max Puncta Size: Upper limit to remove any extremely large particles
     - Noise tolerance/prominence: this value is the minimum value needed to differentiate to maxima from eachother (if it does not meet this criteria, they are grouped together). A good value is typically 5-10% of the average thresholding value for your control condition (e.g. 50-100 for 1000)
  - Measurements 
    - Length: A .csv with a column of the image file names titles "File" and a column of the lengths titled "Length" This is an output from the MaxMin Macro
    - Thresholding : A .csv with a column of the image file names titles "File" and a column of the thresholding titled "Threshold" This is an output from the Single Channel Thresholding Macro
    - If these are not available, length will be set at 1, and you will be prompted to add a set thresholding value to apply to all images.
   
- Macro will then open an image file, search the thresholding and apply the appropriate value, run find maxima, analyze particles with the set min/max size and prominence, and then generate masks, ROI's and counts of the puncta. Counts will be standardized by matched length and results will be in puncta/10um, based on the appropriate scale.
- Results will include folders containing the thresholding masks, ROIs for each image, and excel files:
  - Average Puncta contains results for each image, including puncta density
  - Individual puncta will have a line item for each puncta, including size, signal intensity, centroid locations etc. This is supplemental to the average results
  - Puncta Analysis contains genera data about the experiment/macro run
  - Common errors: Sometimes with assigning the thresholding and length values from the .csv files there is an error.
    - For length, the results of the calculation for puncta per 10um are infinity, and all lengths are 0. In that case, this can be calculate manually (tranfer over the correct lengths and calculte) and the macro does not need to be re-run
    - For thresholding, this is a bigger issue, where the issue is typically due to mismatched file names/indexes for identification. You'll see it upon runnig the macaro as the thresholding is set and segmentation of particles. Checking filenames compared to values within "Label2Array" or the .csv "File" column is the first thing to see. Additionally, if there is a common string (ROIs from the same image) you may accidentally apply the same threshold across the multiple ROIs (can be checked in the average puncta results document by looking at the thresholding value for each line item). Sometimes closing Fiji and starting again can fix if there doesn't seem to be any real reason for the issue.

**6. Colocalization Analysis**
This macro allows you to quantify colocalization of 2 proteins of interest (C1 and C2), which have already been examined for single channel puncta analysis.
- Starting folders (obtained from independent runs of Macro 5):
  - C1 Folder (Masks)
  - C2 Folder (Masks)
- Output Folder (auto-generate "Merge" folder)
- Prompt:
  - Name channels 1, 2
  - Pixel scale
 
- Starting folders (obtained from independent runs of Macro 5):
  - C1 Folder (Masks)
    
- Part 1: Opens [n] files from both C1 and C2 folders, merge, and then saves (note: Macro automatically selects images by their number [n] in filelist alphabetically, so you will want to make sure these numbers match up. worth checking during first attemps to make sure the merged images are using the right match)
- Part 2: Opens merged mask image, splits channels, analyzes particles of C1, then measures the ROIs on C2, and vice versa.

- Output: .csv files
  - Colocalization Summary: notes (Channels, scale)
  - Channel 1 Individual Puncta: C1 ROIs measures on C2
  - Channel 2 Individual Puncta: C2 ROIs measures on C1

- Results will give you for individual puncta:
  - sizes (scaled)
  - percent colocalization (how much of the ROI is positive for signal from the other channel. typically >25% is considered colocalization
    

**7. Puncta Nearest Neighbor Analysis**
This macro allows you to use the Individual Puncta CSVs for two different markers from the Single Channel Puncta Analysis and run a nearest neighbor analysis. For a given set of puncta from a staining (e.g. Homer1 puncta at postsynapses), you can find the distance to the nearest puncta from a different staining (e.g. Bassoon puncta at presynapses) within the same ROI. This allows you to look at the distances between different puncta, which is often important for understanding spatial relationships

Inputs
- Two Individual Puncta CSVs from 5. Single Channel Puncta Analysis
  - CSVs should be from two different stains - you are looking at the distance between two different sets of puncta
  - The order in which you upload the CSVs is important. The first CSV is the query while the second CSV is the reference. The code looks at each query puncta and finds the nearest reference puncta to it
  - Make sure you are using the version of 5. Single Channel Puncta Analysis in this fork, which collects centroid location
  - The macro will use the names of each entry to only analyze nearest neighbor of puncta recorded from the same image

Outputs
- A new CSV is saved in the same folder as the query puncta
- Each query punctum's centroid location and image name is listed, as well as the distance to the nearest reference punctum and the centroid location and image name of that query punctum
- Centroid locations and distances are shown in microns



