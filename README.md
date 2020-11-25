## this branch is already good for 2017 analysis, 

## please make all changes for 2016 in the present branch such that this can be used for both years. 

## make all the new switches configurable via python file, DO NOT MAKE C++ ONLY SWITCHES ANYMORE. 

# Installation of ExoPieElement and dependencies
New README


### setup CMSSW

export SCRAM_ARCH=slc7_amd64_gcc700   

cmsrel CMSSW_10_6_17

cd CMSSW_10_6_17/src

cmsenv

git cms-init

git cms-merge-topic cms-egamma:EgammaPostRecoTools #just adds in an extra file to have a setup function to make things easier

git clone -b particleNet_2017_2016 git@github.com:ExoPie/ExoPieElement.git

scram b -j 4 


##For jetToolBox

git clone git@github.com:cms-jet/JetToolbox.git JMEAnalysis/JetToolbox

cd JMEAnalysis/JetToolbox

git checkout jetToolbox_102X_v3

cd -

scram b -j 4


cd ExoPieElement/TreeMaker

config file to run is in test dir: named treeMaker_16_17_cfg.py 

cd test 

## before cmsRun do set the proxy. 

cmsRun treeMaker_16_17_cfg.py runOnMC=True runOn2017=True


## for crab submission 

go the the directory: 

cd ExoPieElement/CrabUtilities

copy the latest .py file from test dir, do it via

source setup.sh


edit the crabConfig_2017_MC.py file, change following parameters as per your site of storage: 

config.Data.splitting = 'EventAwareLumiBased'
config.Data.unitsPerJob = 30000

config.Site.storageSite = "T2_TW_NCHC"                                                                                                                                                                      
config.Data.outLFNDirBase = '/store/group/phys_exotica/bbMET/ExoPieElementTuples/%s' %(workname)

you can make a list of samples in a text and use it to submit multiple jobs using MultiCrab_2017MC.py

you need to edit the .txt file name and run it using 

python MultiCrab_2017MC.py

 to check the status of all the jobs you just submitted you can add one function in the same file to do it. there is some example in 




for crab submit: 
python MultiCrab_2017MC.py --submit 

for crab status: 
python MultiCrab_2017MC.py --status --crabdir=crab_MC_2017miniaodV2_V1

for crab status with summary of all the dataset:  (ss refer to status summary)
python MultiCrab_2017MC.py --status --crabdir=crab_MC_2017miniaodV2_V1 --ss 

for crab resubmit: 
python MultiCrab_2017MC.py --resubmit --crabdir=crab_MC_2017miniaodV2_V1

for crab kill: 
python MultiCrab_2017MC.py --kill --crabdir=crab_MC_2017miniaodV2_V1

