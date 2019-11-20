# Implement-the-K-means-algorithm-
implement the k-means algorithm 
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans
import math
from scipy.spatial import distance
import random
import csv

#Get birth rates and life expectancy

def read_data_file(file_address, column1, column2):
    
   csv_file = pd.read_csv(file_address)
   
   xy_data = csv_file[[column1, column2]]
   
   data_list = xy_data.astype(float).values.tolist()
    
   return data_list

#Get countries

def get_countries(file_address):
    
   csv_file = pd.read_csv(file_address)
   
   countries_column = csv_file[['Countries']]
   
   countries_list = countries_column.astype(str).values.tolist()
    
   return countries_list


#user to define dataset

file_to_read = 0

while file_to_read <1 or file_to_read >3:

   file_to_read = int(input("Enter the number of the file you want to read:\n"
    
                            "1. Data from 1953\n"
                            
                            "2. Data from 2008\n"
                            
                            "3. Data from both 1953 and 2008\n"))
    
   if file_to_read == 1:
   
       data_list = read_data_file(r"data1953.csv",'BirthRate(Per1000 - 1953)','LifeExpectancy(1953)')
       
       countries_list = get_countries(r"data1953.csv")
        
   elif file_to_read == 2:
   
       data_list = read_data_file(r"data2008.csv",'BirthRate(Per1000 - 2008)','LifeExpectancy(2008)')
       
       countries_list = get_countries(r"data2008.csv")
        
   elif file_to_read == 3:
   
       data_list = read_data_file(r"dataBoth.csv",'BirthRate(Per1000)','LifeExpectancy')
       
       countries_list = get_countries(r"dataBoth.csv")
        
   else:
   
       print("Invalid option.")


#user to define parameters for data plot

n_clusters = int(input("Number of clusters required: "))

n_iterations = int(input("Iterations required: "))

#x data = birth rate / y data = life expectancy

kmeans = KMeans(n_clusters = n_clusters, max_iter = n_iterations) 

kmeans.fit(data_list)

centroids = kmeans.cluster_centers_

cluster_name = kmeans.labels_

dist = kmeans.inertia_


#Setting plot colours

colours =  ['b.', 'g.', 'r.', 'c.', 'm.', 'y.', 'k.', 'w.']

for i in colours:
    
   while 2 > len(colours):
   
       colours.append(i)
        
data_clustered = []

just_values =[]


for i in range(len(data_list)): 

    data_clustered.append([countries_list[i],data_list[i],str(cluster_name[i])])  
    
    just_values.append([data_list[i],str(cluster_name[i])])
    
    plt.plot(data_list[i][0],data_list[i][1],colours[cluster_name[i]],markersize=10)


#plotting the data   

plt.scatter(centroids[:,0],centroids[:,1], marker = "X")

plt.xlabel('BirthRate')

plt.ylabel('Life Expectancy')

plt.show()


#list of cluster symbols i.e. 0, 1, 2 etc

cluster_list = []

for i in range(n_clusters):

    cluster_list.append(str(i))

clus_for_values = {key:[] for key in cluster_list}

data_values = clus_for_values.items()

for i in data_values:
    
   for j in just_values:
   
       if i[0] == j[1]: 
       
           i[1].append(j[0])

            
#print(data_values)

clus_dict = {key:[] for key in cluster_list} #attach the cluster list as the keys to the dictionary

clust_data = clus_dict.items() #returns a list of items for each key in the dictionary

for i in clust_data:
    
   for j in data_clustered:
   
       if i[0] == j[2]: 
       
           i[1].append(j[0:2])


#text output to user of data in user's previously defined parameters

print("\n\nCOUNT OF COUNTRIES USED:\n")

for i in clust_data:

    print("There are",(len(i[1])),"countries in cluster",(i[0]))


#Countries in each cluster

for key, value in clust_data: 

    print("\n\nCOUNTRIES IN CLUSTER:",(key[0]),":\n")
    
   for list_ in value:
   
       print(list_[0])


#Displaying birth rate and life expectancy averages

print("\n\nBIRTH RATE AND LIFE EXPECTANCY AVERAGES:\n")

x_birth = []

y_life = []

for key, value in data_values:
    
   for list_ in value:
   
       x_birth.append(list_[0])
       
       y_life.append(list_[1])
        
   meansx = np.mean(x_birth)
   
   print("cluster",(key),":")
   
   print("Birth rate:",(round(meansx,2)))
    
   meansy = np.mean(y_life)
   
   print("Life expectancy:",(round(meansy,2)),"\n")
  

