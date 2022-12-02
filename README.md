# DIS08_PA_Schmitt - Personal Assignment
Personal assignment for the DIS08 module for the winter semester in 2022/23. 

## Student Data

Name: Florian Schmitt\
Matrikelnummer: 11154630

## Application Purpose

This repository contains the entire code developed for an application that provides the user with an administrative map of Germany. It contains information on the supply of charging stations for electrical vehicles compared to the demand represented by the total of registered cars per district. The data used is provided by open-data portals and can be accessed via the links provided in the dataset_description.md file. 

## Content

This repository contains the following files and should be viewed in the suggested order:
1. README.md
2. application_conceptualization.md
3. dataset_description.md
4. methodic_procedure.md
5. application.ipynb
6. insights_limitations.md

In addition to these files, the *data* folder contains datasets for which a live connection could not be implemented, as well as a second README file with further details on why this was not possible.

## Python Environment Setup

The following setup was used to run application.ipynb. Should your environment fail to execute application.ipynb, please use specific installation sequence of libraries to ensure the file is executable without errors:
1. Create new virtual environment (Conda Environment preferred) with Python 3.9
2. Install Jupyter
3. Install Geopandas Version 0.9.0

Please do not update or install a newer version of pandas or matplotlib as these might cause the code to fail. These libraries and its required versions come with the geopandas installation.