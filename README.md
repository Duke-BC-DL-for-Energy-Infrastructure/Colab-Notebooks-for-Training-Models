# Colab-Notebooks-for-Training-Models
## Purpose of this repo
This repo is meant to store any notebooks we use to run experiments with synthetic data.

## Using the notebooks
There are a couple options, including downloading, accessing directly through Google Colab, or cloning the repo.

To download a notebook, go the repo and click on the notebook. Then select raw and then either 1) right click and 'Save Page As...' or 2) press <kbd>Ctrl</kbd> + <kbd>s</kbd> or <kdb>⌘</kbd> + <kbd>s</kbd> on mac and then select a location to save it. Make sure it has the extension '.ipynb'. Then, you can upload it to Google Colab or use as a Jupyter Notebook.

To access through [Google Colab](https://colab.research.google.com/), open up the webiste and a window should appear with recent notebooks listed. Click GitHub on the menu bar and copy and paste this link: https://github.com/Duke-BC-Deep-Learning-for-Energy-Access/Colab-Notebooks-for-Training-Models. The notebooks in the repo should show up and you can then select on to open it. If you want to push to this repo after editing the notebook (also make sure to tell people if you do), go to File -> Save a Copy in GitHub and then change the commit message to be descriptive. The information for the repo, branch, and name should be correct (unless you want to push to a different branch). Once you hit 'OK', the changes should be made on the remote repo for everyone else to see.

Alternatively, you could clone the repo on your local computer or on a remote machine and then push it after you edit it.

## Organization
The goal is to keep this repo clutter-free and simple to use. That likely means having only one notebook (which would be the most up-to-date version) per experiment. Make sure to either tell everyone what you are going to push or make a pull request so the whole team is aware of what changes are being made.

## Experiments
### Main Experiment
The main experiment is training the YOLOv3 object detection model on a baseline dataset of overhead images of wind turbines, then adding synthetic overhead images of wind turbines to the training set, and observing how the performance of the model changes. The performance is evaluated using a testing set that remains the same. The baseline dataset and supplemental synthetic images are located [here](https://figshare.com/projects/Adding_Synthetic_Imagery_for_Object_Detection_on_Overhead_Images_of_Wind_Turbines/96131).

The images for the basline dataset were collected from [NAIP Imagery](https://www.fsa.usda.gov/programs-and-services/aerial-photography/imagery-programs/naip-imagery/) using primarily the [Power Plant Satellite Imagery Dataset](https://figshare.com/articles/dataset/Power_Plant_Satellite_Imagery_Dataset/5307364). Additional images were collected using [Earth OnDemand](https://earthondemand.astraea.earth/). The wind turbines were hand-labeled in each image to be the ground truth.

For the synthetic images, we use CityEngine to uniformly generate models of wind turbines on top of background images. The background images are NAIP images collected from [Earth OnDemand](https://earthondemand.astraea.earth/). We use a script to randomly pick background images and generate models, and then position the virtual camera overhead in multiple locations and save the images. This process is repeated (with the same seed) without background images and where the wind turbine models are colored black to generate ground truth labels for the synthetic images.

### Second Experiment
We are also trying to look at the performance of the model on different types of wind turbines. In California and some of Arizona, there are relatively small wind turbines that are much more difficult to locate from overhead imagery. For this experiment, we are trying to observe the baseline performance on both small and large wind turbines and then see how both of those performances change when we add in synthetic imagery. In addition, we can add models of small wind turbines to our synthetic imagery to our training set and see how the performance on small wind turbines change.
