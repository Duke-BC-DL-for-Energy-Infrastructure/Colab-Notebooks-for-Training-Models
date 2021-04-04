# Colab-Notebooks-for-Training-Models
## Purpose of this repo
This repo is meant to store any notebooks we use to run experiments with synthetic data.

## Using the notebooks
There are a couple options, including downloading, accessing directly through Google Colab, or cloning the repo.

To download a notebook, go the repo and click on the notebook. Then select raw and then either 1) right click and 'Save Page As...' or 2) press <kbd>Ctrl</kbd> + <kbd>s</kbd> or <kdb>⌘</kbd> + <kbd>s</kbd> on mac and then select a location to save it. It should already have it, but make sure the file being saved has the extension '.ipynb'. Then, you can upload it to Google Colab or use as a Jupyter Notebook.

To access through [Google Colab](https://colab.research.google.com/), open up the webiste and a window should appear with recent notebooks listed. Click GitHub on the menu bar and copy and paste this link: `https://github.com/Duke-BC-DL-for-Energy-Infrastructure/Colab-Notebooks-for-Training-Models`, and then click somewhere else on the pop-up window. The notebooks in the repo should show up and you can then select on to open it. If you want to push to this repo after editing the notebook (also make sure to tell people if you do), go to File -> Save a Copy in GitHub and then change the commit message to be descriptive. The information for the repo, branch, and name should be correct (unless you want to push to a different branch). Once you hit 'OK', the changes should be made on the remote repo for everyone else to see.

Alternatively, you could clone the repo on your local computer or on a remote machine and then push it after you edit it.

## Organization
The goal is to keep this repo clutter-free and simple to use. That likely means having only one notebook (which would be the most up-to-date version) per experiment. If it's a major change, make sure to tell the team what you're pushing so everyone is aware.

## Experiments
### Change in Performance when Adding Synthetic Data for Cross and Within Domain - Regional Constrained New Background Imagery + New Syn Gen
We split the available images into different regions, and picked three of them to use for this experiment. Here, we train on 100 images from a particular region and then validate on 100 images from a region (which could be the same as the training region). Since we are using three different regions, we can train on three different regions and validate on three different regions, giving nine pairs of training and validation regions. We then add synthetic data that is meant to replicate the validation domain and observe the performance change. This synthetic data is created using background imagery that was collected adjacent to the locations of the images in our validation set. By doing this, we can see if this improves the synthetic data versus when the synthetic data didn't use adjacent background imagery. This informs us about how important the background imagery is in our synthetic data. Due to the variance between different training runs on the same training data, we repeat each condition (baseline vs. adding synthetic) four times.

### Optimal Ratio of Synthetic Images - Ratio Test New Background Imagery + New Syn Gen
For each pair of training and validation regions out of our three regions, we can adjust the ratio of real to synthetic imagery to try to find an optimal ratio. To do this, we vary the number of synthetic imagery while keeping the number of real images constant at a value of 100. Here, we run ratios of 1:0 (baseline / no synthetic imagery), 1:0.5, 1:1, and 1:2. In the notebooks in Regional Constrained New Background Imagery + New Syn Gen, we use a ratio of 1:0.75 so that also gives us information about the optimal ratio.

### Change in Performance when adding Synthetic Data - Wind Turbine Object Detection with Ultralytics Yolov3.ipynb
This was our very first experiment, and uses all of the available data in the experiment. This experiment involves training the YOLOv3 object detection model on a baseline dataset of overhead images of wind turbines, then adding synthetic overhead images of wind turbines to the training set, and observing how the performance of the model changes. The performance is evaluated using a testing set that remains the same. The baseline dataset and supplemental synthetic images are located [here](https://figshare.com/projects/Adding_Synthetic_Imagery_for_Object_Detection_on_Overhead_Images_of_Wind_Turbines/96131).

The images for the basline dataset were collected from [NAIP Imagery](https://www.fsa.usda.gov/programs-and-services/aerial-photography/imagery-programs/naip-imagery/) using primarily the [Power Plant Satellite Imagery Dataset](https://figshare.com/articles/dataset/Power_Plant_Satellite_Imagery_Dataset/5307364). Additional images were collected using [Earth OnDemand](https://earthondemand.astraea.earth/). The wind turbines were hand-labeled in each image to be the ground truth.

For the synthetic images, we use CityEngine to uniformly generate models of wind turbines on top of background images. The background images are NAIP images collected from [Earth OnDemand](https://earthondemand.astraea.earth/). We use a script to randomly pick background images and generate models, and then position the virtual camera overhead in multiple locations and save the images. This process is repeated (with the same seed) without background images and where the wind turbine models are colored black to generate ground truth labels for the synthetic images.

### Change in Performance on Small vs. Large Wind Turbines when adding Synthetic Data - Multiclass_Wind_Turbine_Object_Detection_with_Ultralytics_Yolov3.ipynb
We are also trying to look at the performance of the model on different types of wind turbines. In California and some of Arizona, there are relatively small wind turbines that are much more difficult to locate from overhead imagery. For this experiment, we are trying to observe the baseline performance on both small and large wind turbines and then see how both of those performances change when we add in synthetic imagery. In addition, we can add models of small wind turbines to our synthetic imagery to our training set and see how the performance on small wind turbines change.

### Impact of Synthetic Imagery with Varying Dataset Sizes - Downsized_Wind_Turbine_Object_Detection_with_Ultralytics_Yolov3.ipynb
This experiment looks at the impact of adding synthetic imagery with different sized datasets. We took the original training dataset of 1239 real images and removed some of the images to get training datasets of 1/8, 1/4, 1/2, and 3/4 the size of the original one. Then we repeat the experiment of comparing the baseline results (just real imagery) with the results when adding the 441 synthetic images to the training dataset for each of the smaller datasets.

### Utility of Synthetic Imagery for Cross Domain Testing - Categorical Cross Domain
This experiment is to analyze the helpfulness of synthetic imagery when there is a lack of training data in a region of interest, where the model would want to be deployed. Here we train on images of deserts and then test the model on images of farmlands. Then, we add synthetic images that have background images of farmland into the training set. Our training set then contains real images of deserts and synthetic images of farmlands, and we again test the model on the real images of farmlands. After testing both models, we can compare results and see whether the synthetic imagery helped the model adapt to the unseen geographic domain.

We are also repeating this same experiment, but with the training and validation domains flipped, so that the model is training on farmland and validating on desert. Then we add synthetic images of desert into the training set to see how the performance changes.
