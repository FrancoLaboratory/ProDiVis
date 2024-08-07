# ProDiVis
A Z-stack validation suite written in the python programming language, designed to be user-friendly for those with little knowledge of image analysis or programming. Using 'Section-specific Intensity Normalization' (SsIN), ProDiVis normalizes fluorescent signal from a signal of interest using a normalization signal deemed likely to have similar mean fluorescent intensity across focal plane depth in the signal of interest. SsIN-normalized are then heatmapped using 'Section-Normalized Intensity Projection' (SNIP) to visualize protein distribution before and after normalization. ProDiVis accepts standard file extension (`.tiff`, `.png`, `.jpg`) outputs generated from bioformat files (for example: CARL ZEISS `.czi` or Leica `.lif`) of any bit-depth. 

Heatmaps can be generated from any fluorescent Z-tack and serve as an unbiased visualization tool. While it benefits from its computational and financial accessibility to all labs, ProDiVis assumes that fluorescent scattering in the normalization signal and the signal of interest are similar enough to use the section-specific intensity of the former to normalize the intensity of the latter. Ultimately, it is up to the user to determine whether their samples meet this assumption.

## Inherent Problems with Confocal Deep Imaging
A common problem with laser scanning confocal microscopy is the intensity of excitation light and emission signal from fluorophores decrease as they travel through 3D specimens, resulting in weaker signal reaching the detector. To facilitate image interpretation, we developed proDiVis: a visualization algorithm involving focal-plane-specific signal normalization. However, there are limitations to optical sectioning while imaging thick specimens, namely fluorescence intensity loss as a function of imaging depth. The most widely used fluorophores have emission light in the visible spectrum that can only penetrate a limited distance through biological material. This is a known contributor to the fundamental depth limit, caused by several physical properties such as light scattering and absorption. To address the above issues, we developed a computational method to proportionally compare pixel values across the depth of 3D specimens, accounting for the decrease in fluorescence intensity we and others have previously observed. While there are many existing programs or software available for image analysis, to the best of our knowledge, none of them account for loss of signal by normalizing against a housekeeping signal, and many of them are expensive and/or require a high level of technical expertise to use.

## Dependencies
```
ipython==8.12.3
ipywidgets==8.1.3
jupyterlab==4.2.4
matplotlib==3.8.4
numpy==2.0.1
opencv-python==4.9.0.80
pandas==2.2.2
scipy==1.14.0
```

`Data_Pipeline.ipynb` contains all the necessary code to run proDiVis

# Detailed Installation Instructions for Novice Users
We reccomend using python 3.11.3 (latest release as of April 5, 2013) which can be found
[here](https://www.python.org/downloads/)

We run proDiVis and subsequently python through [Anaconda](https://www.anaconda.com/), which is a software management tool designed to simplify the process of working with python and many python packages required for scientific computing. Using either method of installation works.

Navigate to the directory (folder) where you wish to install proDiVis

## *i*. Cloning ProDiVis

Using the command line: `git clone https://github.com/FrancoLaboratory/ProDiVis.git`

You should have downloaded all the files needed to run proDiVis inside this folder. Before running proDiVis, you must create a virtual environment and install the required packages. 

## *ii*. Creating the Virtual Environment

### **ii**-a. Using pip to create an environment
#### Create a virtual environment for ProDiVis.
Navigate to the directory where ProDiVis was installed. Use the following command to create a virtual environment for ProDiVis. Replace 'env_name' with the desired name of your virtual environment.

`python -m venv env_name`

#### Activate the environment.
After activation, your chosen environment name should appear in parentheses at the beginning of every line in your shell.

Windows: `.\env_name\Scripts\activate`

Linux or macOS: `source env_name/bin/activate` 

#### Install required packages

`pip install -r <prodivis_repository_path>\requirements.txt`

### *ii*-b. Using conda to create an environment
Conda is a third-party package management software. It offers advantages in its extensive documentation, robust implementation, and easy integration into a variety of other applications like VSCode, which will be discussed later. If you are a super user, you may even consider using [Mamba](https://mamba.readthedocs.io/en/latest/installation/mamba-installation.html), a version of Conda with a C++ backend, allowing for quicker package downloads and faster switching between environments. 

#### Install Conda
Open [this link] (https://conda.io/projects/conda/en/latest/user-guide/install/index.html) in your brower for conda installation instructions.

**NOTE**: You will need to make sure that the `condabin` folder installed by the installer is on your system's PATH. In Windows, this is typically done through the system variables registry [tutorial here](https://stackoverflow.com/questions/44272416/how-to-add-a-folder-to-path-environment-variable-in-windows-10-with-screensho). On Mac/Linux, this is typically done through the command line using your systems `$PATH` variable [tutorial here](https://osxdaily.com/2014/08/14/add-new-path-to-path-command-line/)

#### Navigate to the directory (folder) where ProDiVis was installed.

`conda env create -f environment.yaml` 

#### Activate the environment

`conda activate prodivis-env`

# Operation Instructions
### 1. Setting up images and filesystem
Before running the code, please ensure your filesystem consists of a parent directory with the ProDiVis repository and any dataset repositories cloned within. An example filesystem could look like the following:


`📁<parent_directory>`
```
└── 📁ProDiVis
    └──📁Scripts 
        └── Data_Pipeline.ipynb
        └── *.py
    └──📁data
        └──📁signal_stack
        └──📁normalization_stack
```

`└── 📁ProDiVis_Images` <span style="color:green"><em># clone if desired</em></span>

The repositories containing the glioblastoma and mouse heart datasets can be found at the links below:
 - [**ProDiVis_Images**](https://github.com/FrancoLaboratory/ProDiVis-Images)

### Creating your own dataset for analysis / visualization by ProDiVis
Assuming that the images are already in a bioformat, export the images in .tiff format into a directory of your choosing. Ensure all filenames are `imgname_ZX.tiff` where `imgname` is the name of the image and `X` is the Z-stack number and focal plane number respectively. For example, `['tumoroid_img24_Z01.tiff', 'tumoroid_img24_Z02.tiff', ...]`. Images should be saved in the `data` directory within the `ProDiVis` directory downloading using git during setup. Within the data directory, the signal of interest (that you would like heatmapped by SNIP) should be in the `signal_stack` subdirectory. The normalization stack (used by SsIN) should be in the `normalization_stack` directory. 

### 2. Open `Data_Pipeline.ipynb`
There are two ways to run the `Data_Pipeline.ipynb`:
1. **Jupyter Lab (fully supported)**: In your shell, type `jupyter lab`. A browser window should open, with a file directory on the left. Double-click on `Data_Pipeline.ipynb` to open it.
2. **[VSCode](https://code.visualstudio.com/) (ipywidget functionality reduced)**: First ensure that VSCode is [installed](https://code.visualstudio.com/Download). After installation, there are two ways to open a jupyter notebook (any `.ipynb`) in VSCode.
	- Open your file browser (File Explorer on Windows, Finder on Mac), and navigate to the folder where ProDiVis was installed. Right-click and select `VSCode` under the _open\_with_ menu.
	- In your shell, type `code .`. A new VSCode window should open, with the files in the ProDiVis folder on the left. Double-click on `Data_Pipeline.ipynb` to open it.

**NOTE**: If opening in VSCode, you will need to ensure that the python kernel used during runtime comes from the vertual environment you set up using `venv` or `conda`. [This tutorial](https://code.visualstudio.com/docs/datascience/jupyter-kernel-management) from Microsoft may help. If you're still running into issues, you could try:
 - [This tutorial](https://datascienceharp.medium.com/how-to-get-vs-code-to-recognize-your-conda-environment-461c79354297) for `conda` managed environments.
 - [This tutorial](https://medium.com/@marcio.debarros/a-seamless-transition-setting-up-virtual-environment-and-jupyter-notebooks-in-vs-code-6debf9078ddd#:~:text=Select%20the%20Kernel&text=Click%20%E2%80%9CSelect%20Kernel%E2%80%9D%20in%20the,environment%20and%20the%20Recommended%20one.) if using `venve` managed environments. 

### 3. Run the first cell to ensure that all necessary libraries are properly imported

### 4. In the second cell, in quotes:
insert the path to your signal of interest (SOI) that will be assigned to the `stack_dir` variable
insert the path to your normalization signal (NS) that will be assigned to the `norm_dir` variable
insert the name of your soi and ns and assign them to the `soi` and `ns` variables, respectively

### 5. Determine whether to limit optical sections in stack virualization
The `zmin` and `zmax` variable corresopnd to the minimum and maximum Z-stack number to be included in analysis. For example, if you have a Z-stack with 10 focal planes and you only want to analyze the first 5, `zmin` would be 1 and `zmax` would be 5. If you want to analyze all 10 focal planes, assign `None` to both variables

### 6. Understanding `z_multiplier`
proDiVis will generate orthogonal projections of your Z-stack, meaning you will view your Z-stack in x, y, and z planes, simultaneously. Increasing `z_multiplier` will multiply the pixels of your Z-stack in the z direction, meaning that the z plane will be stretched. This is useful if you want to visualize your SOI in the z direction.

### 7. Determine whether to save figures
If you want to save your figures, assign `True` to the `save_figs` variable. If not, assign `False`. We reccomend running through the `Data_Pipeline.ipynb` notebook once without saving figures to ensure that the analysis is correct.

### 8. Adjusting lower and upper thresholds
Normalization by proDiVis begins with histogram thresholding, a technique that segments an image by setting a range of pixel intensity values to be considered for analysis. The user is required to select a lower and upper boundary which correspond to the minimum and maximum pixel intensity values. ProDiVis excludes any pixel value outside of the user-defined boundaries. We reccomend setting starting with `lower_thresh = 0` and `upper_thresh = 254`. This will include all pixel values that are not saturated in the analysis/normalization

### 9. Scaling images
You will reach a section of `Data_Pipeline.ipynb` that says "Image Rescaling". This section allows you to look at the normalized images and apply a scaling factor to brighten them, this step is optional. A graphical user interface will display in the file if you run `tools.stack_gui`.

If you choose to rescale images, they will be saved in a new directory adjacent to your directory that contains the input images.

In the next cell, the path to the scaled images will need to be typed and assigned to the `scaled_norm_stack_tiffs` variable (similar to step 4)

### 10. Hit run all cells!
The rest of the `Data_Pipeline.ipynb` file is designed to work seamlessly without the need for any more user input. After the variables described above have been changed, you are free to run the entire notebook.
