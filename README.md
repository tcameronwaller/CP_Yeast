# CP_Yeast
Pipelines in CellProfiler for processing fluorescence microscopic images of yeast

I prepared the program pipeline for "yeast_cell_mito_2-flour.cpproj" in CellProfiler (http://cellprofiler.org/). CellProfiler has multiple tutorials (http://cellprofiler.org/tutorials/) to help users to learn its basic operation. CellProfiler also has a manual (http://cellprofiler.org/manuals/) with information in more detail. CellProfiler provides many types of modules that give basic functionality for processing images. It also provides a Graphical User Interface (GUI) to enable the user to use these modules easily. The user sets parameters to customize these modules and then assembles multiple modules together to form a complete program or pipeline. CellProfiler's manual (http://cellprofiler.org/manuals/) provides thorough information on the function of each module. This information is important for customizing modules and assembling new program pipelines.

----------

# Use Case

Here is the experimental scenario for which I intend this pipeline to be useful.

Cell type:
Saccharomyces cerevisiae (yeast)
The program assumes that the regions of interest (cellular regions) appear circular.

Magnification:
1000-X (100-X objective X 10-X eyepiece)
The program assumes that the regions of interest (cellular regions) appear circular within a specific range of radii, appropriate for yeast at a magnification of 1000-X.

Channels:

1) Refraction
This channel is for some refractory image of cells. By refraction, I refer to methods such as Bright-Field, Phase-Contrast, and Differential-Interference-Contrast (DIC). I developed the program for images in DIC, but other filters or techniques might also work. The program uses this channel to recognize cellular regions. Acquiring images in a focal plane that shows clear borders around cellular regions can improve the results of this process. These borders need to be some sort of intensity gradient, such as shadows.

2) Mitochondrial Marker
This channel is for some fluorescent marker for mitochondria. The program specifically looks for mitochondria with a tubular structure.

3) Study Marker
This channel is for some fluorescent marker for the protein of interest.

----------

# Process Steps

The program pipeline processes images in multiple steps. I combined all of these steps into a single pipeline. As pipelines in CellProfiler are modular, the user can easily select modules from the pipeline to perform just the functions necessary.

1) Image Extraction (Modules _-_)
Extract image information from raw files. The microscope's software saves image information in a raw format. In our case that format is Carl Zeiss Image (.czi) format. Cell Profiler uses BioFormats (http://www.openmicroscopy.org/site/products/bio-formats) to accomodate many raw image formats.

2) Intensity Scale (Modules _-_)
Rescale intensities of images to fit the bit-depth of the new format. I set the program to convert image information from the raw format to tiff with a 16-bit depth. Rescaling can improve the visibility of signal in images. Otherwise it can be too dark to see. It is very important not to rescale the intensity of individual images if you want to quantify the actual intensity and compare it between images in a batch. I designed the program to use both images with rescaled intensity (for visibility) and with original intensity (for quantification).

3) Region Segmentation (Modules _-_)
Segment images to identify or recognize regions of interest. The program first segments regions corresponding to cells. The program then segments regions corresponding to mitochondria within those cells.

4) Measurement (Modules _-_)
Measure images by regions of interest. The program measures the areas of regions of interest and the intensities of fluorescent channels within those regions. The program also measures the correlation of intensity in both fluorescent channels within cellular regions.

5) Export (Modules _-_)
Export measurements in multiple tables with tab-delimiters.

----------
