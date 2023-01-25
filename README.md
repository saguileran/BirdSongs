<a href="https://github.com/saguileran/birdsongs/"><img src="./assets/img/logo.png" width="500"></a>


# birdsongs

A python package for analyzing, visualizing and generating synthetic bird songs from recorded audio.


[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/saguileran/birdsongs/main?labpath=BirdSongs.ipynb)


#  Table of Contents
<!---
- [birdsongs](#birdsongs)
- [Table of Contents](#table-of-contents)
--->
- [Overview](#overview)
- [Objective](#objective)
- [Repository Contents](#repository-contents)
  - [Physical Model](#physical-model)
  - [Programming Object Oriented (POO)](#programming-object-oriented-poo)
  - [Implementation](#implementation)
- [Installation](#installation)
  - [Requirments](#requirments)
  - [Downloading](#downloading)
- [Use](#use)
  - [Define](#define)
  - [Solve](#solve)
  - [Visualize](#visualize)
  - [Note](#note)
- [Results](#results)
- [References](#references)
  - [Literature](#literature)
  - [Software](#software)
  - [Audios Dataset](#audios-dataset)
---
  
  
# Overview

Study and packaging of the physical model of the **motor gestures for birdsongs**. This model explains the physics of birdsongs by modeling the organs involved in sound production in birds (syrinx, trachea, glottis, Oro-Oesophageal Cavity (OEC), and beak) with oridary differential equations (ODEs). In this work, a Python package is developed to analyze, visualize and generate synthetic birdsongs using the motor gestures model and recorded samples of birdsongs. To automate the model, the problem is formulated as a minimization problem with three control parameters (air sac pressure of the bird’s bronchi, labial tension of the syrinx walls, and scale constant) and solved using numerical methods, signal processing tools, and numerical optimization. The package is tested and generated comparable syllables. The minimization problem is solved using recorded samples of some bird syllables of different species (Zonotrichia capensis, Ocellated Tapaculo, and mimus gilvus) that are downloaded from Xeno-Canto and eBird,  the solution of the optimization problem generates a synthetic syllable for comparison of the fundamental frequency (denoted as FF, F0, or also called pitch) and the spectral conent index (SCI) of both synthetic and real syllables.

# Objective

Design, development, and evaluation of a computational-physical model to generate synthetic birdsongs from recorded samples.

# Repository Contents

This repository contains the documentation, scripts, and results necessary to achive the proposed objective. The files and information are divided in branches as follows:

- **main:** Python package with tutorial examples, necessary data, and some results.
- **dissertation:** Latex document bachelor's dissertation: *Design, development, and evaluation of a computational physical model to generate synthetic birdsongs from recorded samples*.
- **gh-pages:** Archieves for the [BirdSongs](https://saguileran.github.io/birdsongs/) website, a more in-depth description of the package.
- **results:** Some results obtanied from the tutorial examples: image, audios and motor gesture parameters (.csv).

The model used, Motor Gestures for birdsongs [1], have been developed by profesog G. Mindlin at the [Dynamical Systems Laboratory](http://www.lsd.df.uba.ar/) (in spanish LSD) of the university of Buenos Aires, Argentina. 

## Physical Model 

Schematic implementation of the physical model **motor gestures for bydsongs**. It models the syrinx, trachea, glotis, OEC, and beak with ODEs. 

<p align="center"> <img src="./assets/img/model.png" width="700" title="model"></p>

## Programming Object Oriented (POO)

Taking advantege of POO the repetition of long codes is avoid. Using this programming paradigm, the execution and implementation of the model is fast and easy. Five objects ared created to solve the optimization problem and analyze the synthetic syllables:

- **Syllable**: define a object from audio syllable with its tempo and spectral features
- **Optimizer**: define a object to optimize function from method and syllables
- **BirdSong**: define a object to read and split an audio song in syllables 
- **Plot**: define a object to plot real and synthec syllables or songs
- **Paths**: define a object to organize folders location

In order to understand the diagram methodology, the following icons will be used. 

<p align="center">  <img src="./assets/img/objects.png" width="500" alt="methodology"></p>

Each icon is an object with different tasks. The majoe advantage of this implementation is the possiblity to easily compare the features and characteristics of syllables (birdsongs or chuncks) between objects. 

## Implementation

Using the previous objects defined, the optimization problem is solved by following the next diagram steps

<p align="center">  <img src="./assets/img/methodology.png" width="600" alt="methodology"></p>

The final output is a parameters object with the optimal control parameters coefficients of the motor gesture found.

  
# Installation

## Requirments

`birdsong` is implemented in python 3.8 and requires:


- librosa
- lmfit
- scipy
- sympy
- numpy
- pandas
- matplotlib
- playsound
- PeakUtils
- mpl_pan_zoom
- mpl_point_clicker
- scikit_learn
- scikit_maad
- setuptools
- ipython
- pygobject
- ffmpeg

    
## Downloading

In order to use birdsongs, clone the repository and enter to the folder repository

```bat
git clone https://github.com/saguileran/birdsongs.git
cd birdsongs
```
you can verify the current branch with the command `git branch -a`. You have to be in `main` branch, to change the branch use the command `git checkout main`.

The next step is to install the required packages, any of the following commands lines will work

```bat
pip install -r ./requirements.txt
# python -m pip install -r ./requirements.txt  # (equivalent)
```
<!--
You can now use the package in a python terminal opened at the birdsongs folder. 

To use the package from any folder install the repository, this can be done with any of the two following lines
-->

Install the birdsong package

```bat
python .\setup.py install
```

That's all! 
   
<!---
and then add to python 

```bat
pip install -e birdsongs
python -m pip install -e birdsongs
```
-->

Take a look at the tutorials notebooks for basic uses: physical model implementation, [motor-gestures.ipynb](./tutorials/motor-gestures.ipynb); define and generate a syllable from a recorded birdsong, [syllable.ipynb](./tutorials/syllable.ipynb); or to generate a whole birdsong, several syllables, [birdsong.ipynb](./tutorials/birdsong.ipynb),

# Use

## Define

Import the package as `bs` 

```python
import birdsongs as bs
```  
  
Define the ploter and paths objects, optionally you can specify the audio folder or enable to save figures 

```python
# audios = "path\to\audios"     # default examples/audios/
# root = "path\to\audios"       # default ./
# bird_name = "path\to\audios"  # default None

ploter = bs.Ploter(save=True)  # images are save at ./examples/results/Images/
paths  = bs.Paths()            # root, audios_path, bird_name
```

Displays the audios found with the `paths.AudiosFiles()` function, if the folder has a *spreadsheet.csv* file this functions displays all the information about the files inside the folder.

**BirdSong**
  
Defining and plotting the wave sound and spectrogram of a birdsong object

```python
birdsong = bs.BirdSong(paths, no_file=0, NN=1024, umbral_FF=1.0,
                       #tlim=(t0,tend), flim=(f0,fmax) # other features
                      )
ploter.Plot(birdsong, FF_on=False)  # plot the wave sound and spectrogram
birdsong.Play()                     # in notebook useAudioPlay(birdsong)
```

**Syllables**
  
Define the syllables using time intervals of interest from the whole birdsong. You can choose the points with the `ploter.Plot()` function by changing the value of `SelectTime_on` to `True`
    
```python
ploter.Plot(birdsong, FF_on=False, SelectTime_on=True) # selct 
time_intervals = Positions(ploter.klicker)             # save 
time_intervals                                         # displays

syllable = bs.Syllable(birdsong, tlim=time_intervals[0], NN=birdsong.NN, Nt=30,
                       umbral_FF=birdsong.umbral_FF, ide="syllable")
ploter.Plot(syllable, FF_on=True);
``` 
  
## Solve
  
The last step consists in defining the optimizer object to generate the synthetic syllable (song), solving the optimization problem. For example, to generate the synthetic syllable (or birdsong) with the previously defined time intervals 

```python
brute_kwargs = {'method':'brute', 'Ns':11}          # optimization mehotd. Ns is the number of grid points
optimizer    = bs.Optimizer(syllable, brute_kwargs) # optimizer object
optimal_gm   = optimizer.OptimalGamma(syllable)     # find optimal gamma 

optimizer.OptimalParams(syllable, Ns=11)            # find optimal parameters coefficients
#syllable, synth_syllable = optimizer.SongByTimes(time_intervals)   # find optimal parameters over the time intervals
```
    
define the optimal synthetic syllable object with the values found above

```python
synth_syllable = syllable.Solve(syllable.p)
```

## Visualize
  
Visualize and write the optimal synthetic audio 
    
```python
ploter.Plot(synth_syllable);                # sound wave and spectrogram of the synthetic syllable
ploter.PlotVs(synth_syllable);              # physical model variables over the time
ploter.PlotAlphaBeta(synth_syllable);       # motor gesture curve in the parametric space
ploter.Syllables(syllable, synth_syllable); # synthetic and real syllables
ploter.Result(syllable, synth_syllable);    # scoring variables and other spectral features

birdsong.WriteAudio();  synth_syllable.WriteAudio(); # write both audios at ./examples/results/Audios
```
  
## Note  
  
To generate a single synthetic syllable (chunck) you must have defined a birdsong (syllable), the process is as follows:

1. Define a paths object.
2. Use the previous path obeject to define a birdsong (syllable) object, it also requeries the file number (birdsong for a syllable). Here you can define the window FFT length and the umbral threshold to compute the pitch 
3. Define an optimization object with a dictionary of the method name and its parameters.
4. Find the optimal gamma, for a single syllable or for a set of syllables defined from time intervals.
5. Find the optimal labia parameters, the motor gesture curve.
6. Generate the synthetic birdsong from the previous control parameters found.
7. Plot and save all the syrinx, scoring, and result variables.
8. Write the syllable audios defined both synthetic and real.
<!--
```python
syllable  = bs.Syllable(birdsong)           # additional options: flim=(fmin,fmax), tlim=(t0,tend) 

brute     = {'method':'brute', 'Ns':11}     # define optimization method and its parameters
optimizer = bs.Optimizer(syllable, brute)   # define optimizer to the syllable object

optimizer.optimal_gamma                     # find teh optimal gamma over the whole bird syllables
obj = syllable                              # birdsong or chunck
optimizer.OptimalParams(obj, Ns=11)         # find optimal alpha and beta parameters
    
Display(obj.p)                              # display optimal problem parameters
obj_synth_optimal = obj.Solve(obj.p)        # generate the synthetic syllable with the optimal parameters set
    
ploter.Syllables(obj, obj_synth_optimal)    # plot real and synthetic songs, sound waves and spectrograms
ploter.PlotAlphaBeta(obj_synth_optimal)     # plot alpha and beta parameters in function of time (just syllable has this attributes)
ploter.Result(obj, obj_synth_optimal)       # plot the spectrograms, scores and features of both objects, the real and synthetic
    
bird.WriteAudio();  synth_bird.WriteAudio() # write both objects, real and synthetic
```
-->
    
The repository has some audio examples, in [./examples/audios](https://github.com/saguileran/birdsongs/tree/main/examples/audios) folder. You can download and store your own audios in the same folder or enter the audio folder path to the Paths object, the package also has a function to download audios from Xeno-Canto: birdsong.util.DownloadXenoCanto().

The audios **must** be in WAV format or birdosngs will not import them, we suggest use [Audacity](https://www.audacityteam.org/) to convert the audios without any problem.

 
# Results

The model is tested with different syllables of the birdsong of the rufous collared sparrow . Results are located at [examples/examples](./examples/results), images and audios. For more information visit the project website [birdsongs](https://saguileran.github.io/birdsongs/) and enter to [results](https://saguileran.github.io/birdsongs/results/) page. 

Simple syllable of a birdsong of the Rufous Collared Sparrow

<p align="center">  <img src="./examples/results/ScoresVariables-syllable-4_short_FINCA153_Zonotrichia_capensis_trimed.wav-0.png" width="1000" alt="methodology"></p>

Simple syllable of a birdsong of the Ocellated Tapaculo - Acropternis

<p align="center">  <img src="./examples/results/ScoresVariables-syllable-XC104508 - Ocellated Tapaculo - Acropternis orthonyx.wav-0.png" width="1000" alt="methodology"></p>

<!--
<center>
  Zonotrichia capensis - XC11293 <br>
  <audio src="\examples\results\audios\XC11293 - Rufous-collared Sparrow - Zonotrichia capensis.wav-syllable-0-synth.wav" controls preload></audio>
  <audio src="\examples\results\audios\XC11293 - Rufous-collared Sparrow - Zonotrichia capensis.wav-syllable-0.wav" controls preload></audio>
  
  Euphonia Laniirostris Crassirostris - C541457 <br>
  <audio src="\examples\results\audios\C541457 - Thick-billed Euphonia - Euphonia laniirostris crassirostris.wav-syllable-synth-0.wav" controls preload></audio>
  <audio src="\examples\results\audios\C541457 - Thick-billed Euphonia - Euphonia laniirostris crassirostris.wav-syllable-0.wav" controls preload></audio>
  
</center>
-->

The PDF documentof the bachelor's thesis is stored in the `dissertation` brach of this repository, <a href="https://github.com/saguileran/birdsongs/blob/dissertation/main.pdf">Design, development, and evaluation of a computational physical model to generate synthetic birdsongs from recorded samples</a>. 

<p align="center">
  <img src="https://github.com/saguileran/birdsongs/blob/gh-pages/assets/img/cover.jpg" width="300" height="400">
  <img src="https://github.com/saguileran/birdsongs/blob/gh-pages/assets/img/under-cover.png" width="300" height="400">
</p>


# References

## Literature

<div class="csl-entry">[1] Amador, A., Perl, Y. S., Mindlin, G. B., &#38; Margoliash, D. (2013). Elemental gesture dynamics are encoded by song premotor cortical neurons. <i>Nature 2013 495:7439</i>, <i>495</i>(7439), 59–64. <a href="https://doi.org/10.1038/nature11967">https://doi.org/10.1038/nature11967</a></div>
<br>

## Software

<div class="csl-entry">[2] Newville, M., Stensitzki, T., Allen, D. B., &#38; Ingargiola, A. (2014). <i>LMFIT: Non-Linear Least-Square Minimization and Curve-Fitting for Python</i>. <a href="https://doi.org/10.5281/ZENODO.11813">https://doi.org/10.5281/ZENODO.11813</a></div>
<br>

<div class="csl-entry">[3] Ulloa, J. S., Haupert, S., Latorre, J. F., Aubin, T., &#38; Sueur, J. (2021). scikit-maad: An open-source and modular toolbox for quantitative soundscape analysis in Python. <i>Methods in Ecology and Evolution</i>, <i>12</i>(12), 2334–2340. <a href="https://doi.org/10.1111/2041-210X.13711">https://doi.org/10.1111/2041-210X.13711 Dataset </a> </div>
<br>

<div class="csl-entry">[4] McFee, B., Raffel, C., Liang, D., Ellis, D. P., McVicar, M., Battenberg, E., & Nieto, O. &#38; (2015). librosa: Audio and music signal analysis in python. <i>  In Proceedings of the 14th python in science conference </i>, <i>12</i>(12), (Vol. 8). <a href="https://librosa.org/doc/latest/index.html">Librosa</a> </div>

## Audios Dataset

<div class="csl-entry">[5] Xeno-canto Foundation and Naturalis Biodiversity Center &#38; (2005). <a href="https://xeno-canto.org/">xeno-canto:</a>     <i> Sharing bird sounds from around the world.</i> <i>  <a href="https://xeno-canto.org/set/8103">Dissertation Audios Dataset </a> </i></div>
<br>

<div class="csl-entry">[6] Ther Cornell Lab of Ornithology &#38; (2005). ebird <i>, <a href="https://ebird.org/">ebird.com</a> </div>

    
