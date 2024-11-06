# Naturalistic Arm Movements

## Overview of Data
This dataset contains Electrocorticographic (ECoG) recordings of naturalistic arm movements collected from 12 participants over multiple days. The data captures spontaneous, unstructured wrist movements contralateral to the implanted hemisphere, providing a naturalistic insight into motor control mechanisms. The dataset is formatted in MNE-Python epochs, including ECoG signals, electrode locations, and behavioral/environmental metadata.

**Note:** For the course I only considered a single subject during one day.

### Data Collection Protocol
Participants were monitored in a clinical setting with subdural ECoG electrodes sampled at 1000 Hz. Continuous video recordings (30 fps) accompanied each 24-hour monitoring session, enabling precise synchronization with movement events. The wrist movements were tracked using a machine-learning-based pose estimation model to identify initiation events, focusing on the contralateral wrist.

**Note:** The subject I considered was implanted with 94 electrodes and the original data provided was resampled to 500 Hz.

![Electrodes Position for Subject 1 (Peterson et al., 2020)](./figures/electrodes_position.png)

### Movement Synchronization
Each wrist movement event was synchronized with the ECoG data through timestamp alignment. Movement initiation events were determined by changes in wrist state, transitioning from rest to movement, and were defined based on radial displacement from the initial position. This setup allowed for detailed tracking of wrist position, reach angle, movement speed, and whether the movement was unimanual or bimanual.

## Loading Nix Data File
The daset in Nix format is located in `data/nix_files`. 

To load it, first create the Conda environment using the [environment.yml](https://gin.g-node.org/YoshuaLima/naturalistic_arm_movements/src/master/environment/environment.yml) file with the command:

```
conda env create -f environment.yml
```

There are two different options to load the data:

### Option 1
To load and visualize the AnalogSignals, use the [analog_data_visualization.ipynb](https://gin.g-node.org/YoshuaLima/naturalistic_arm_movements/src/master/codes/analog_data_visualization.ipynb) notebook provided in the `codes` folder.

**Note:** Some data units, such as arm movement coordinates (units: px, sampling frequency: fps), differ from those provided by the `quantities` library. Define these custom units before loading the Nix file:

```
px = pq.UnitQuantity('px', pq.dimensionless, symbol='px')    
fps = pq.UnitQuantity('fps', pq.Hz, symbol='fps')
```

![Example of ECoG Epoch](./figures/epoch_plot.png)

### Option 2
Additionally, it is also possible to use the [behaverioral_data_visualization.ipynb](https://gin.g-node.org/YoshuaLima/naturalistic_arm_movements/src/master/codes/behaverioral_data_visualization.ipynb) notebook to load the Nix file and then visualize the features of the behavioral/environmental metadata.

![**Distribution of the behavioral/environmental metadata for Subject 1.** This figure is based on a figure sowed in the original reference (Peterson et al 2020)](./figures/metadata_plot.png)

## Metadata and Variables
The Nix file is structured with a primary block containing multiple segment and encapsulating the complete dataset for a single subject recorded over one day. The main block includes annotations for key variables such as `subject` (subject number), `day` (recording day), `no_channels` (number of channels), and `ch_names` (channel names). This block contains 156 segments, each representing a recorded epoch and annotated with essential metadata to provide detailed context for the data.



| Variable | Explanation |
|-----------------|-----------------|
| time | Time when the movements and epoch start and end |
| event_timestamp | Used to synchronize the coordinates of the wrist and the ECoG epochs |
| mvmt | Wrist moved (ECoG electrodes were implanted on the hemisphere of the opposite side) |
| vid_name | Filename of the video recorded |
| event_frame_idx | Frames video index |
| false_pos | False positives of the wrist movement, these should not be considered |
| patient_id | ID of the patient (other than the number of subject) |
| I_over_C_ratio | Ratio of the ipsilateral wrist reach magnitude to the sum of ipsilateral and contralateral reach magnitudes |
| audio_ratio | Audio ratio used to analyze how it affects the ECoG signal |
| reach_duration | Time that the subject takes to reach the maximum radial displacement of the wrist |
| reach_r | Magnitude of the reach |
| reach_a | 2D reach angle (angles were converted from 90-270° to range from 90° to -90°) |
| onset_velocity | Wrist marker radial speed during movement onset |
| other_reach_overlap | Temporal overlap between contralateral and ipsilateral move states over the duration of the entire contralateral wrist movement |
| bimanual | Clasification of movements (bimanual or unimanual)  based on the amount of temporal lag between contralateral and ipsilateral wrist movement onset|


## Creating New Nix Data File
The steps to create a new Nix data file are:

1. Download the original FIF file from a different subject or day, and save it in the `data/fif files` directory. The link to the original dataset is provided in the references.
2. Implement the `nix_file_creation.ipynb` notebook located in the `codes` folder.
3. The file created will be storaged in `data/nix_files` folder, retaining the same original file name with a `.nix` extension.

The data structure of the new file will match the structure described in the `Loading Nix Data File` section

## References  

**Original repository:** Peterson, Steven (2020). Naturalistic reach ECoG data. figshare. Dataset. https://doi.org/10.6084/m9.figshare.12115728.v1

**Original publication:** Steven M. Peterson, Satpreet H. Singh, Nancy X. R. Wang, Rajesh P. N. Rao and Bingni W. Brunton
eNeuro 24 May 2021, 8 (3) ENEURO.0007-21.2021; https://doi.org/10.1523/ENEURO.0007-21.2021

# naturalistic_arm_movements
