How to run analysis_R.script
============================
* Please put the run_analysis.R is in the working directory 
* Please unzip the data for the poject in the working directory folder "test" and "train"
* Make sure the run_analysis.R and data for the project (such as folder "test", "train" and other files) are all in the working directory, for example, the screenshot below.

![image](https://raw.githubusercontent.com/wargamer1988/GetData/master/folder.PNG)

In the window of R, enter the following command to get the tidy dataframe and data file: 
```
source("run_analysis.R")
```
### Here are the list of important dataframe and files.
* dataframe "final_data": tidy data set from step1-4
* dataframe "tidydata_sum": summarized tidy data set from step 5
* file "gacd-w3-data1.txt": tidy data file from step1-4
* file "gacd-w3-data2.txt": tidy data file from step5
