# HiForestProducerTool

This repository hosts a collection of simple examples that use CMSSW EDAnalyzers to extract Trigger information and produce a ROOT file from the CMS public heavy-ion data collected in 2010. Here you will find instructions on how to run these codes and reproduce the analysis of the dimuon spectrum.

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.10606131.svg)](https://doi.org/10.5281/zenodo.10606131)

## Instructions

### Preparing the Container

To perform this analysis, we will use the [Docker container](http://opendata.cern.ch/docs/cms-guide-docker). First, download and install Docker as shown in the link above. After that, open a terminal and run the following command:

  ```
  docker run --name hi2010_od -it  gitlab-registry.cern.ch/cms-cloud/cmssw-docker/cmssw_3_9_2_patch5-slc5_amd64_gcc434:latest /bin/bash
  ```

Once the container is running, follow these steps:

- Create a working directory and clone the repository:

  ```
  mkdir HiForest
  cd HiForest
  git clone -b 2010 https://github.com/thiagorangel45/HIOpenDataAnalysis.git HiForestProducer
  cd HiForestProducer
  ```

- Compile the code:

  ```
  scram b
  ```

### Running the Configuration File

The configuration file is set to run only `100` events by default. This is just to verify that everything works properly. If no errors occur and a ROOT output file is produced correctly, you can change `100` to `-1` in the configuration file to run over all events.

To run the configuration:

  ```
  cmsRun hiforestanalyzer_cfg.py
  ```


This configuration reads input ROOT files listed in: `CMS_HIRun2010_HIAllPhysics_ZS-v2_RECO_file_index.txt`. After running, a file named `HiForestAOD_DATAtest.root` will be created as output.

**Note:** The first time you run the command, it may take a long time (depending on your internet speed), and it might seem like nothing is happening — that is normal. You may need to split the input file list and process each file separately. In this case, always change the output file name to avoid overwriting.

To merge several ROOT output files into one:

```
hadd <final_output_name> <arquivo_1> <arquivo_2> ....
```

This will produce a file named `final_output_name.root` (you can choose any name you like).

You can also edit the file [src/Analyzer.cc](src/Analyzer.cc) to include additional objects like tracks, electrons, etc., in the HiForest output. Instructions are provided within the source file itself.

### Running the Analysis

The file [forest2dimuon.C](forest2dimuon.C) is a ROOT script that analyzes the output file. It applies a trigger filter and performs basic selection and histogramming of the invariant mass.

In the folder [forest2dimuon](forest2dimuon), you can find modifications to the original file and the resulting plots.

You can also find some variations of this script in the `hi2010` directory.

To run the script, make sure you have [ROOT](https://root.cern/install/) installed. Then execute:

```
root -l forest2dimuon_2010PbPb_mass.C
```

This will generate a plot like the one below:

<p align="center">
  <img src="forest2dimuon/diMuon_mass_2010_PbPb_1.png" alt="DiMuon Invariant Mass Plot" width="700">
</p>

You can explore other triggers for your analysis by opening the ROOT file with the ROOT browser using: `TBrowser b` Then navigate through the Trigger tree to inspect the available paths.
