# HiForestProducerTool

This repository hosts a collection of simple examples that use CMSSW EDAnalyzers to extract Trigger information and produce a ROOT from the CMS public heavy-ion and proton-proton collision data collected in 2011. Here you will find instructions on how to run these codes and reproduce the dimuon spectrum analysis.

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.10606247.svg)](https://doi.org/10.5281/zenodo.10606247)

## Instructions

### Preparing the Container

To perform this analysis, we will use the [Docker container](http://opendata.cern.ch/docs/cms-guide-docker). Download and install Docker following the instructions at the link. After that, run the following command in your terminal:

  ```
  docker run --name hi2011_od -it  gitlab-registry.cern.ch/cms-cloud/cmssw-docker/cmssw_4_4_7-slc5_amd64_gcc434:latest /bin/bash
  ```

Once the container is running, follow these steps:

- Create a working directory and clone the repository:

  ```
  mkdir HiForest
  cd HiForest
  git clone -b 2011 https://github.com/thiagorangel45/HIOpenDataAnalysis.git HiForestProducer
  cd HiForestProducer
  ```

- Compile the code:

  ```
  scram b
  ```


### Running the Configuration File

The configuration file is set to run only `100` PbPb events to verify that the code runs correctly. If no errors occur and the ROOT output file is generated, you can change `100` to `-1` to run over all events.

Run the configuration with:

  ```
  cmsRun hiforestanalyzer_cfg.py
  ```

This configuration reads the input ROOT files listed in: `CMS_HIRun2011_HIDiMuon_RECO_04Mar2013-v1_root_file_index.txt`


An output file named `HiForestAOD_DATAtest.root` will be created.

**NOTE:** The first run may take a long time (depending on your connection speed), and it might seem like nothing is happening — that's normal. You might need to split the input file list and process them one at a time. In that case, always change the output file name to avoid overwriting.

To merge all output files into a single one, use:

```
hadd nome_do_arquivo_final arquivo_1 arquivo_2 ....
```

This will create a file called `final_output_name.root` (you can choose any name). You can also edit [src/Analyzer.cc](src/Analyzer.cc) to include other objects like tracks, electrons, etc., in the HiForest output. Instructions are provided within the file.

### Running the Analysis

The file [forest2dimuon/forest2dimuon.C](forest2dimuon/forest2dimuon.C) is a script to analyze the output file. It applies a trigger filter and performs a basic selection and invariant mass histogramming. In the [forest2dimuon](forest2dimuon) folder, you can see modifications to the original file and the resulting plots.

To run the script, you will need [ROOT](https://root.cern/install/) installed. Once ROOT is installed, run:
```
root -l forest2dimuon_2011PbPb_mass.C
```

This will produce a plot like:

<p align="center">
  <img src="forest2dimuon/PbPb/diMuon_mass_2011_PbPb_1.png" alt="2011 PbPb DiMuon Mass Plot" width="700">
</p>

You can select other triggers for your analysis by opening the ROOT file with `TBrowser b` in ROOT and browsing the Trigger tree.

### Running the Analysis for Proton-Proton Data

To analyze the proton-proton reference collisions, you only need to:

- Change the input files
- Update the JSON file
- Replace `datasetName = cms.string("HIDiMuon")` with `datasetName = cms.string("AllPhysics2760")` in the `hiforestanalyzer_cfg.py` file

Then repeat the previous steps and run the corresponding script:
```
root -l forest2dimuon_2011pp_mass.C
```

This will produce a plot like:

<p align="center">
  <img src="forest2dimuon/pp/diMuon_mass_2011_pp_1.png" alt="2011 pp DiMuon Mass Plot" width="700">
</p>

