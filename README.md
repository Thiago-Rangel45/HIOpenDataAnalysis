### Preparing the Container:

We will use a Docker container to perform this analysis. You can download Docker from the following link: [Docker container](https://www.docker.com/products/docker-desktop/). After downloading and installing Docker, open your terminal and run the following command:

  ```
  docker run -it --name hi2013_od -P -p 5901:5901 -p 6080:6080 -v ${HOME}/hi2013_od:/code/hi2013_od gitlab-registry.cern.ch/cms-cloud/cmssw-docker-opendata/cmssw_5_3_20-slc6_amd64_gcc472 /bin/bash
  ```

After downloading and entering the container, you will see the folders from this repository. Navigate to the [test](HeavyIonsAnalysis/JetAnalysis/test) directory, where you will find some Python scripts. However, we will download a slightly modified script using the following command (this is not a completely new script, just a small modification of `runForest_pPb_Data_53X.py`):

```
wget https://raw.githubusercontent.com/cms-opendata-validation/HeavyIonDataValidation/53X/runForest_pPb_DATA_53X_OD.py

```

### Running the Script:

```
cmsRun runForest_pPb_DATA_53X_OD.py
```
  
You will receive some error and processing messages, but it should still work and produce a ROOT file named `HiForest.root`. This ROOT file is available in this repository inside the [test](HeavyIonsAnalysis/JetAnalysis/test) folder, and you can download just the ROOT file if you prefer to inspect the trees and branches. Everything should work fine for jets up to this point.

### Muon Tree Issue

Since our analysis focuses on the muon channel, we attempted to include the muon tree by uncommenting line 234 in the `runForest_pPb_DATA_53X_OD.py` file and running the code again. However, we encountered a problem where the muon tree remains empty, while all other trees are filled and function correctly.

The output is a ROOT file available at [HiForest.root](HeavyIonsAnalysis). You can download and open it using ROOT’s TBrowser to inspect the contents.
