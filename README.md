# clusteringcustomers

---

## Table of Contents

- [Description](#description)
- [Date-Time Library](#date-time-library)
- [Customer Data Clustering Library](#customer-data-clustering-library)
  - [How To Use](#how-to-use)
  - [List of Function](#list-of-function)

---

## Description

The project broadly aims at developing two kinds of utilities-  
- First, to import all date-time data in a standard format ( YY-mm-dd HH:MM:SS)  
- Second, to cluster Customer data (to import all kinds of customer data and perform clustering over them. The clusters are plotted and tabulated so that it could be further used to gt insights about the data.)  
These utilities were later used to cluster realtime anonymised customer data attached here.  

---

## Date-Time Library

It imports all kinds of datatime series and convert into a standard *" YY-mm-dd HH:MM:SS "* format.  
Example- 10/10/2019 --> 2019-10-10  
         10/10/2019 10:35:35 ---> 2019-10-10 10:35:35  
  **Parameters**  
    -file_name - file to be imported  
    -col_list - list of column names/ column numbers to be imported from the given file  
    -date_time_col - the column name/ column number that has data-time data
    -seperator - the seperator used in the file(eg. ',' for .csv files, ' ' for .txt files, etc)  

---

## Customer Data Clustering Library

## How To Use

- To cluster a few specified group of columns - Call the function [cluster_pair_list](#cluster_pair_list)
- To cluster all possible combinations of columns - Call the function [all_clusters](#all_clusters)

---

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

  ### predict_cluster_by_parameters
  **Parameters**  
    -X - list of parameters whose cluster number needs to be predicted  
    -kmeans_listitem - a list cotaining dataframe and kmeans parameter. Its the list returned by the [cluster_pair_list](cluster_pair_list) function  
    
  It finds cluster to while X belongs and then returns the dataframe containing all datapoints in that cluster.  

  ### predict_cluster_by_ID
  **Parameters**  
    -id_ - a string ID(can be customerID, clusterID or hostID)  
    -df - the dataframe that was returned after clustering  
    
  It returns a dataframe containing all the datapoints of the cluster that the customer with the givn ID belongs to.  
    
  ### clust_members_with_labels
  **Parameters**  
    -df - dataframe that has been returned by [cluster_pair_list](cluster_pair_list) function after clustering is done (now the datafram has an extra column 'labels')
    -label - the cluster label whose members are required  
    
  It finds all the data points in the cluster with the given label and returns them in the form of a dataframe.  
  
  ### export_csv
  **Parameters**  
    -df - dataframe to be exported  
    -filename - filename with which the exported dataframe needs to be saved  
    
  It exports the given dataframe and saves it in .csv format.  
    
---

[Back To The Top](#clusteringcustomers)
