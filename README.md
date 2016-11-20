# **CP_Yeast**

Repository address: https://github.com/tcameronwaller/cp_yeast

Pipelines in CellProfiler for processing fluorescence microscopic images of yeast

I prepared the program pipeline for "yeast_cell_mito_2-flour.cppipe" in CellProfiler (http://cellprofiler.org/). CellProfiler has multiple tutorials (http://cellprofiler.org/tutorials/) to help users to learn its basic operation. CellProfiler also has a manual (http://cellprofiler.org/manuals/) with information in more detail. CellProfiler provides many types of modules that give basic functionality for processing images. It also provides a Graphical User Interface (GUI) to enable the user to use these modules easily. The user sets parameters to customize these modules and then assembles multiple modules together to form a complete program or pipeline. CellProfiler's manual (http://cellprofiler.org/manuals/) provides thorough information on the function of each module. This information is important for customizing modules and assembling new program pipelines.

----------

## **Use Case**

Here is the experimental scenario for which I intend this pipeline to be useful.

### **Cell type:**

Saccharomyces cerevisiae (yeast)

The pipeline assumes that the regions of interest (cellular regions) appear circular.

## **Magnification:**

1000-X (100-X objective X 10-X eyepiece)

The pipeline assumes that the regions of interest (cellular regions) appear circular within a specific range of radii, appropriate for yeast at a magnification of 1000-X.

## **Channels:**

**1) Refraction (REF)**

This channel is for some refractory image of cells. By refraction, I refer to methods such as Bright-Field, Phase-Contrast, and Differential-Interference-Contrast (DIC). I developed the program for images in DIC, but other filters or techniques might also work. The program uses this channel to recognize cellular regions. Acquiring images in a focal plane that shows clear borders around cellular regions can improve the results of this process. These borders need to be some sort of intensity gradient, such as shadows.

**2) Marker (MRK)**

This channel is for some fluorescent marker for mitochondria. The program specifically looks for mitochondria with a tubular structure.

**3) Target of Interest (TAR)**

This channel is for some fluorescent marker for the target of interest, probably a protein with a fluorescent label. The experiment usually explores the sub-cellular abundance and/or location of this target.

----------

## **Process Steps**

The program pipeline processes images in multiple steps. I combined all of these steps into a single pipeline. As pipelines in CellProfiler are modular, the user can easily select modules from the pipeline to perform just the functions necessary.

**1) Image Extraction (Modules 1-7)**

Extract image information from raw files. The microscope's software saves image information in a raw format. In our case that format is Carl Zeiss Image (.czi) format. Cell Profiler uses BioFormats (http://www.openmicroscopy.org/site/products/bio-formats) to accomodate many raw image formats. The pipeline reads intensity information from the images and then saves these in TIF format with 16-bit depth in a sub-directory with name "Extraction". It saves images for separate channels in separate directories, "REF", "MRK", and "TAR", within the "Extraction" directory.

**2) Intensity Scale (Modules 8-13)**

Rescale intensities of images to fit the bit-depth of the new format. I set the program to convert image information from the raw format to tiff with a 16-bit depth. Rescaling can improve the visibility of signal in images. Otherwise it can be too dark to see. It is very important not to rescale the intensity of individual images if you want to quantify the actual intensity and compare it between images in a batch. I designed the program to use both images with rescaled intensity (for visibility) and with original intensity (for quantification). The pipeline saves these images in TIF format with 16-bit depth in a sub-directory with name "Extraction". It saves images for separate channels in separate directories, "REF_Scale", "MRK_Scale", and "TAR_Scale", within the "Extraction" directory.

**3) Region Segmentation, Cell (Modules 14-188)**

The pipeline segments images in the REF channel to identify or recognize regions of interest corresponding to cells. The pipeline saves images with outlines of these cellular regions for quality control as well as the regions themselves. It saves these in a sub-directory with name "Segmentation_Cell".

**4) Region Segmentation, Mitochondrion (Modules 189-208)**

The pipeline segments images in the MRK channel to identify or recognize regions of interest corresponding to mitochondria. The pipeline associates these mitochondrial regions with the cellular regions that contain them. The pipeline saves images with outlines of these mitochondrial regions for quality control as well as the regions themselves. It saves these in a sub-directory with name "Segmentation_Mitochondrion".

**5) Measurement (Modules 209-211)**

The pipeline measures the areas of regions of interest and the intensities of fluorescent channels within those regions. The pipeline also measures the pixel-by-pixel correlation of intensities in both fluorescent channels (MRK, TAR) within cellular regions.

**6) Export (Module 212)**

The pipeline exports measurements for several regions of interest in multiple separate tables.

----------

## **Process Instructions**

It is necessary for the user to set the default directory, "Default Input Folder" in CellProfiler (File Menu, Preferences) before running the pipeline. The pipeline will create multiple sub-directories within this master directory.

It is also necessary for the user to load a set of images and identify appropriate channels for the Refraction (REF), Marker (MRK), and Target (TAR) channels.

----------

## **Results**

The pipeline exports measurements for several regions of interest in multiple separate tables. These tables are in text (txt) format with tab-delimiters. The user can open the tables in a text editor or import information from the tables into a spreadsheet program by recognizing tab delimiters.


