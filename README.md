# suite2p <img src="logo/logo_unshaded.png" width="250" title="sweet two pea" alt="sweet two pea" align="right" vspace = "50">

Pipeline for processing two-photon calcium imaging data.  
Copyright (C) 2018  Howard Hughes Medical Institute Janelia Research Campus  

suite2p includes the following modules: 

* Registration
* Cell detection
* Spike detection 
* Visualization GUI

This code was written by Carsen Stringer and Marius Pachitariu.  
For support, please open an [issue](https://github.com/MouseLand/suite2p/issues).
The reference paper is [here](https://www.biorxiv.org/content/early/2017/07/20/061507). 

## Installation
From a command line terminal, type:
~~~~
pip install suite2p
~~~~
If you don't already have Python (and pip), you'll need to first install a distribution of Python like [Anaconda](https://www.anaconda.com/download/). Choose Python 3.x for your operating system. You might need to use an anaconda prompt if you did not add anaconda to the path.

## Getting started

The quickest way to start is to open the GUI from a command line terminal. You might need to open an anaconda prompt if you did not add anaconda to the path. 
~~~~
python -m suite2p
~~~~
Then: 
1. File -> Run suite2p (or shortcut ctrl+r)
2. Setup a configuration for your own data
    - -> Add directory to data_path (can be multiple folders, but add them one at a time)
    - -> Add save_path ((otherwise the data directory is used as save path))
    - -> Add fast_disk (this is where the binary file of registered data will be created, choose an SSD for this path) ((otherwise the save path is used as the fast disk path))
    - Set some parameters (see full list below). At the minimum: 
		~~~~
		nplanes, nchannels, diameter, tau, fs
		~~~~
3. Press run and wait. Messages should start appearing in the embedded command line. 
4. When the run is finished, the results will open in the GUI window and there you can visualize and refine the results (see below).

## How to use the GUI

suite2p output goes to a folder called "suite2p" inside your save_path, which by default is the same as the data_path. If you ran suite2p in the GUI, it loads the results automatically. Otherwise, load the results with File -> Load results. 

The GUI serves two main functions:

1. Checking the quality of the data and results. 
	* there are currently several views such as the enhanced mean image, the ROI masks, the correlation map, and the ROI+neuropil traces
	* we will later add more views such as population-level visualizations. 
2. Classify ROIs into cell / not cell (right and left views respectively) 
	* the default classifier included should work well in a wide variety of scenarios. 
	* this classifier can learn from manual curation, and in this way adapt to the statistics of your own data. 

Main GUI controls (works in all views):

1. Pan  = Left-Click  + drag  
2. Zoom = (Scroll wheel) OR (Right-Click + drag)
3. Full view = Double left-click OR escape key
4. Swap cell = Right-click on the cell
5. 
 
## Other ways to call Suite2p

1. From the command line:
~~~~
python -m suite2p --ops <path to ops.npy> --db <path to db.npy>
~~~~
	
2. From Python/Jupyter
~~~~python
from suite2p.run_s2p import run_s2p
ops1 = run_s2p(ops, db)
~~~~

## Outputs

~~~~
F.npy: array of fluorescence traces (ROIs by timepoints)  
Fneu.npy: array of neuropil fluorescence traces (ROIs by timepoints)  
spks.npy: array of deconvolved traces (ROIs by timepoints)  
stat.npy: array of statistics computed for each cell (ROIs by 1)  
ops.npy: options and intermediate outputs
~~~~

## Option defaults

~~~~python
'save_path0': [], # default is the first item in data_path
'diameter':12, # this is the main parameter for cell detection
'tau':  1., # this is the main parameter for deconvolution
'fs': 10.,  # sampling rate (total across planes)
'nplanes' : 1, # each tiff has these many planes in sequence
'nchannels' : 1, # each tiff has these many channels per plane
'functional_chan' : 1, # this channel is used to extract functional ROIs (1-based)
'align_by_chan' : 1, # when multi-channel, you can align by non-functional channel (1-based)
'look_one_level_down': False, # whether to look in all subfolders when searching for tiffs
'baseline': 'maximin', # baselining mode
'win_baseline': 60., # window for maximin
'sig_baseline': 10., # smoothing constant for gaussian filter
'prctile_baseline': 8.,# smoothing constant for gaussian filter
'neucoeff': .7,  # neuropil coefficient
'neumax': 1.,  # maximum neuropil coefficient (not implemented)
'niterneu': 5, # number of iterations when the neuropil coefficient is estimated (not implemented)
'maxregshift': 0.1, # max allowed registration shift, as a fraction of frame max(width and height)
'reg_tif': False, # whether to save registered tiffs for manual inspection
'subpixel' : 10, # precision of subpixel registration (1/subpixel steps)
'batch_size': 200, # number of frames per batch
'num_workers': 0, # 0 to select num_cores, -1 to disable parallelism, N to enforce value
'num_workers_roi': -1, # 0 to select number of planes, -1 to disable parallelism, N to enforce value
'nimg_init': 200, # subsampled frames for finding reference image
'navg_frames_svd': 5000, # max number of binned frames for the SVD
'nsvd_for_roi': 1000, # max number of SVD components to keep for ROI detection
'max_iterations': 10, # maximum number of iterations to do cell detection
'ratio_neuropil': 3., # minimum ratio between neuropil radius and cell radius
'tile_factor': 1, # use finer (>1) or coarser (<1) tiles for neuropil estimation
'threshold_scaling': 1, # adjust the automatically determined threshold by this scalar multiplier        
'inner_neuropil_radius': 2, # number of pixels to keep between ROI and neuropil donut
'outer_neuropil_radius': np.inf, # maximum neuropil radius
'min_neuropil_pixels': 350, # minimum number of pixels in the neuropil
'ratio_neuropil_to_cell': 3, # minimum ratio between neuropil radius and cell radius
'allow_overlap': False, # not 100% sure this is being used         
~~~~

## Dependencies
suite2p relies on the following packages (which are automatically installed if missing):
- pyqtgraph 
- PyQt5
- numpy
- scipy
- h5py
- scikit-image
- matplotlib (not for plotting (only using hsv_to_rgb function), should not conflict with PyQt5)

PyQt has known issues with Yosemite Mac OS, but works on more recent Mac versions. The software has been heavily tested on Windows 10 and Ubuntu 18.04, please let us know if you have problems with other operating systems in the issues.

### Logo
Logo was designed by Shelby Stringer and [Chris Czaja](http://chrisczaja.com/).
