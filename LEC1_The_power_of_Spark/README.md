# MapReduce
MR was designed to analyze massive data sets across a cluster. In this Jupyter notebooks, it shows the sense of Hadoop MapReduce, how it works. However, it would be run first locally rather than in a cluster.

The **biggest different** between Hadoop and Spark is that **Spark** tries to do as many calculations as possible in memory, which avoids moving data back and forth across the cluster.
**Hadoop** writes intermediate calculation out to disk, which can be **less efficient**. Hadoop is an older tech than Spark and one of the cornerstone big data tech.

In this tutorial, it includes a file called `songplay.txt`. This is a text file where each line represent a song that was played in the app Sparkify. The MR code will
count how many times each song was played (**how many times the song title appears in the list**)

## MapReduce vs Hadoop MapReduce

MR is a programming technique, Hadoop MR is a specific implementation of the programming technique.

## Summary what happens in the code:
There is a list of songs in `songplay.txt` that looks like the following:

Deep Dreams Data House Rock Deep Dreams Data House Rock Broken Networks Data House Rock etc.....

During the map step, the code reads in the txt file one line at a time, like this:

(Deep Dreams, 1)  
(Data House Rock, 1)  
(Deep Dreams, 1)  
(Data House Rock, 1)  
(Broken Networks, 1)  
(Data House Rock, 1)  
etc.....

Finally, the reduce step combines all of the values by keys and sums the values:  

(Deep Dreams, \[1, 1, 1, 1, 1, 1, ... \])  
(Data House Rock, \[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, ...\])  
(Broken Networks, \[1, 1, 1, ...\]  

With the output 

(Deep Dreams, 1131)  
(Data House Rock, 510)  
(Broken Networks, 828)  
