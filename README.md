This Repository contains the code used for the runtime study
of the paper "Rescue Craft Allocation in Tidal Waters of the North and Baltic Sea" 
by Tom Mucke, Alexander Renneke, Finn Seesemann and Felix Engelhardt. 
The aim is to allocate rescue craft to stations in tidal waters, and it uses the DGzRS as an example.
You're welcome to use and develop it further as long as you include a citation of our paper as reference. If you have any questions, feel free to contact us via tom.mucke@rwth-aachen.de, finn.seesemann@rwth-aachen.de or engelhardt@combi.rwth-aachen.de.

The code is written in Python and uses the Gurobi solver.

# Installation
This gives a short overview on how to install all the necessary software to run the code in this repository. 
It is meant for Windows user. 
Mac is not recommended as QGIS has some big performance issues on Mac from our experience.
Linux was not tested, but we suspect that Linux users will be able to adapt the installation process to their system and fix some of the path issues in the code for QGIS.

## Disclaimer
Please note that we will install several software packages and libraries. 
These are not maintained by us and we cannot guarantee that the installation will work on your system.
We also cannot guarantee the security of the software packages and libraries or the safety of the installation process.
Please ensure that you trust the sources of the software packages and libraries and that you are comfortable with the installation process before proceeding.
Please also read the license of each package before using it, and make sure to comply with the license. 
Additionally remember that this code is also just provided as is and we cannot guarantee anything, please read the license before using the code.

## Copyright
While this code is under the GNU Affero General Public License v3 and the Copyright belong to Tom Mucke and Finn Seesemann, 
the data provided in data/tidal_points is not our property. The data was provided by the Generaldirektion Wasserstraßen und Schifffahrt (GDWS) 
through their website https://www.pegelonline.wsv.de, they own the rights to the data.



## Installation of required software
### Git
We use Git to manage the code. You can download it from the official website: https://git-scm.com/downloads.
Using git you can then clone the repository to your local machine.


### QGIS
We use QGIS to work with geodata. You can download it from the official website: https://qgis.org/en/site/forusers/download.html.
We tested the code with QGIS 3.34.1, if you use a different version, you might need to adjust some of the paths in the code.
We strongly recommend not using any of the LTR versions if you are not familiar with QGIS, as they require even more adjustments to the code.

### Python
We use Python to run the code. You can download it from the official website: https://www.python.org/downloads/. If not
specified otherwise, all of the code needs to be executed from the root of the project to prevent path issues.

This code was tested with Python 3.11.2. 

#### Python Packages
We recommend to use a virtual environment to install the necessary packages.
A guide on venv can be found under https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/.
The necessary packages can be installed from the requirements.txt file after activating the virtual environment:
```
pip install -r requirements.txt
```


### Gurobi
We use Gurobi to solve the optimization problem. 
In the requirements.txt file we already listed the package pyguorbi, which is a wrapper for the Gurobi solver.
As the instances are quite large the free pyguorbi license is not sufficient. In order of running these large instances you need to get a license from Gurobi.

This code was tested with Gurobi 10.0.0.


## Provided data
Some of the data is provided in the data folder, as it is likely to be changed in the future.
This includes the used tidal data aswell as the ship data. The data of the dgzrs was originally downloaded from
their website, and then extended with additional data, some manually from data sheets, and some from
depth data, please see tidal_gen/get_base_depth.py.


## Getting the remaining data
As some of the data is too large to be stored on GitHub, we decided to download the data from the original sources 
and generate the necessary files from the raw data.

### Tidal Data
The tidal data is taken from Pegel Online (Copyright/Urheberrecht ITZBund). 
The provided data is the one we used for our experiments, but newer 
data can be downloaded from https://www.pegelonline.wsv.de. In order of parsing the already provided data please run 
parse_tidal_points.py. Please make sure to have the root of the project as the working directory, as otherwise the paths in the code will not work.
With the provided data it is expected, that 10843 entries are skipped.

### Zone Data
The zone data will need to be downloaded and then parsed using qgis. The dataset is quite large and will
take some time to download, parse and process. If you just want to run a quick test we provide one file with 100 zones
for seed 1 already, but for proper testing you will need to download the data and parse it yourself.

To do so we will run zone_creator.py. It will create a folder data/geo and download the borders file from 
https://gdz.bkg.bund.de/index.php/default/open-data/geographische-gitter-fur-deutschland-in-utm-projektion-geogitter-national.html. 
Please read their license, check for potential updates and make sure to comply with the license before using the data.

As working with QGIS in Python is quite complex, we will need to run the code using the QGIS Python interpreter.
For QGIS 3.34.1 the interpreter can be found under C:\Program Files\QGIS 3.34.1\apps\Python39\python.exe in Windows. 
If you use a different version of QGIS or a different OS, you will need to adjust the path in the code.
To run using the QGIS Python interpreter, you can use the following command:
```
"C:\Program Files\QGIS 3.34.1\apps\Python39\python.exe" -m zone_creator
```

## Running the code
After installing all the necessary software and getting the data, you can run the code.
Please make sure to have the root of the project as the working directory, as otherwise the paths in the code will not work.

As an easy point of entry, you can run the test.py file. It accepts the parameters as command line arguments or 
will ask for them if not provided. The parameters are the following:
- seed: The seed for the random number generator
- number of zones: The base number of zones
- combined number of zones: The number of zones after k-means clustering
- water: Allows to northern sea or the baltic sea or both
- output: The output file. If specified as a command line argument, the file will be saved to the specified location. If done using the input prompt, the file will be saved to test/result/
- solver: The solver to use


## The Results
For the actual runtime study we used the HPC of the RWTH Aachen University. To do so we used slurm,
which is a job scheduler. The code is not provided in this repository, but we can provide the code if requested.
The code is quite specific to the HPC of the RWTH Aachen University and will need to be adjusted to work on other systems.

We do provide the results of the runtime study in data/results. We do have the
gurobi json dumps, but did not include them here, as they are quite large. If you want to see them, please contact us.


