
# Introduction

This project has been developed as part of a graduate level course(Cloud Computing) at University of Cincinnati.
The main objective of the project is to mine and analyse wikipedia data(in the form of articles) and find top 20 key words in each article based on TFIDF metric

# Explanation

It is a map-reduce chain job which takes input data from HDFS(Hadoop File System) and computes the above mentioned statistics.
The jobs have been developed in JAVA using Map Reduce API and are chained one after another.
All of them are called from the main "Driver" class where Tool Runner class has been used to run the jobs.
The main program takes two command line arguments, one input path and one output path. All the intermediate outputs are handled by the job itself.
Sample output files are present in the "Sample Outputs" folder.

# Approach
All the jobs are run in a HADOOP cluster environment with multiple nodes.
The first job runs on the input data and find term frequencies for each word in each article.
The output from first job is fed into the second job which finds the IDF value and calculate th TFIDF metric for each word in an article.

# Key Customizations
To improve the performance and reduce the running time of the job, custom InputFormat and RecordReader classes have been created.
By default, for a text file(TextInputFormat) each line would act as the key and the byte offset would act as the value.
But since each file is one article and we need article bases statistics, I created a custom input format called WholeFileInputFormat which makes use of a custom record reader class WholeFileRecordReader and reads the entire file(article) as the value and article name as the key.
This not only made the job faster by about 50%, the number of jobs needed were also halved.

#Steps to Run
1. Import the project folder into Eclipse IDE.
2. Export the project as a JAR file by selecting Driver.java as the main class.
3. Upload the jar onto the cluster using a scp or any ftp tool.
4. Assuming HADOOP is running properly on the cluster, go to the folder where the jar is placed and run the below command
``` sh
hadoop jar jarfilename InputFolder finaloutputFolder
```
Note: The finaloutputFolder should not be present already and the Input folder should be a HDFS path.
