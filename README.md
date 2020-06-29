# clusteringcustomers

---

## Table of Contents

- [Description](#description)
- [How To Use](#how-to-use)
- [List of Function](#list-of-function)

---

## Description

The project aims at making various utilities to import all kinds of customer data and perform clustering over them. The clusters are plotted and tabulated so that it could be further used to gt insights about the data.  
These utilities were later used to cluster realtime anonimised custmer data attached here.

---

## How To Use

- To cluster a few specified group of columns - Call the function [cluster_pair_list](#cluster_pair_list)
- To cluster all possible combinations of columns - Call the function [all_clusters](#all_clusters)

## List of Function

### import_data
  **Parameters**  
    -file_name - file to be imported  
    -col_list - list of columns to be imported from the given file  
    -seperator - the seperator used in the file(eg. ',' for .csv files, ' ' for .txt files, etc)  
    
  This function imports the specified columns from the mentioned file and stores them in a dataframe.  
  
### all_clusters
  **Parameters**  
    -df - the dataframe  
    -max_pair_size - maximun size of the combinations of columns of the dataframe to be clustered  
    -n - the number of local closests and farthests for each cluster that needs to be displayed/ plotted  
    -plot_locals - dafault value = False. Bool value to select whether to plot the local closests and farthests for each cluster or just not  
    
  This function takes a dataframe and forms a list of all possible combinations of columns of the dataframe of size less than or equal to *max_pair_size*. Then it another function [cluster_pair_list](#cluster_pair_list) with each of these lists to perform the clustering.  

### cluster_pair_list
  **Parameters**  
    -df - the dataframe  
    -pair_list - list of pairs, triplets,...etc of columns of the dataframe that needs to be clustered together  
    -n - the number of local closests and farthests for each cluster that needs to be displayed/ plotted  
    -plot_locals - dafault value = False. Bool value to select whether to plot the local closests and farthests for each cluster or just not  
    
  This function takes a dataframe and a list of combinations of columns of dataframe to be clustered. It loops through this list and clusters each combination. It calls the function [k_opt_elbow](#k_opt_elbow) to determine the optimum value of k for each case. Then calls three other functions- [local_closests_farthests](local_closests_farthests),  [global_outliers](global_outliers) and [plot_clusters](plot_clusters).  
  
  ### k_opt_elbow
  **Parameters**  
    -df - the dataframe 
    
  Uses elbow method to find the optimum k value for kmeans clustering of the dataset.  
  
  ### local_closests_farthests
  **Parameters**  
    -df - the dataframe  
    -distance - a numpy array containing distance of each data point from each cluster center  
    -labels - a numpy array storing the label of each data point  
    -n - the number of local closests and farthests for each cluster that needs to be displayed/ plotted 
    
  This function takes a dataframe and uses the distance and labels array to find 'n' closests and farthests data point in each cluster. It returns two datasets, one for closest and another for farthest points.  
  
  ### global_outliers
  **Parameters**  
    -df - the dataframe
    -pair - a list of columns on which clustering is done  
    -center - a list of centers of all clusters  
    
  It finds five global outliers amongst the data points and return a dataframe containing them.  
  
  ### plot_clusters
  **Parameters**  
    -df - the dataframe  
    -outliers - dataframe containing global outliers (the one returned bt [global_outliers](global_outliers) function)  
    -local_c, local_f - dataframes containing closest and farthest points in each cluster respetively (the ones returned by [local_closests_farthests](local_closests_farthests) function)  
    -col_list - list of columns of dataframe used for clustering  
    -center - a list of centers of all clusters  
    -labels - a numpy array storing the label of each data point  
    -plot_locals - dafault value = False. Bool value to select whether to plot the local closests and farthests for each cluster or just not  
    
It simply plots the clusters, globals outliers and/or locals closests and local farthests(depending on the value of *plot_local*). It further prints a summary of the plot and a tabulate the local closests and farthests.  

[Back To The Top](#clusteringcustomers)
