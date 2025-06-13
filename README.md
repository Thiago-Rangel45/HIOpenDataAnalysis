### Preparing the Container:

We will use a Docker container to perform this analysis. You can download Docker from the following link: [Docker container](https://www.docker.com/products/docker-desktop/). After installation, open your terminal and run the following command:

  ```
  docker run -it --name hi2015_od -P -p 5901:5901 -p 6080:6080 -v ${HOME}/hi2015_od:/code/hi2015_od gitlab-registry.cern.ch/cms-cloud/cmssw-docker-opendata/cmssw_7_5_8_patch3-     slc6_amd64_gcc491 /bin/bash
  ```

Once the container is downloaded and running, you will see the folders from this repository. Navigate to the [test](HeavyIonsAnalysis/JetAnalysis/test) folder, where you will find some Python scripts. However, we will download a modified version of the script using the command below (this is not an entirely different script — it's just a small modification of `runForestAOD_pp_DATA_75X.py`):

```
wget https://raw.githubusercontent.com/cms-opendata-validation/HeavyIonDataValidation/75X/runForestAOD_pp_DATA_75X_OD.py
```

### Running the Script:
```
cmsRun runForestAOD_pp_DATA_75X_OD.py
```

You will see some error and processing messages, but the script should still run successfully and produce a ROOT file named `HiForest.root`. This file is available in this repository inside the [test](HeavyIonsAnalysis/JetAnalysis/test) folder, and you can download just the ROOT file if you'd like to inspect the trees and branches. Everything should work fine for jet analysis up to this point.

### Muon Tree Issue:

Since our analysis focuses on the muon channel, we tried to include the muon tree by uncommenting line 234 in the `runForestAOD_pp_DATA_75X_OD.py` file and running the code again. However, we encountered an issue where the muon tree remained empty, while the other trees were filled and worked correctly.

The output is a ROOT file available at [HiForest.root](HeavyIonsAnalysis). You can download and open it with ROOT’s TBrowser to inspect the contents.
