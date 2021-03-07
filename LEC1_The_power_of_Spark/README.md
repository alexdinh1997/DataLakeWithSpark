# MapReduce
MR was designed to analyze massive data sets across a cluster. In this Jupyter notebooks, it shows the sense of Hadoop MapReduce, how it works. However, it would be run first locally rather than in a cluster.

The **biggest different** between Hadoop and Spark is that **Spark** tries to do as many calculations as possible in memory, which avoids moving data back and forth across the cluster.
**Hadoop** writes intermediate calculation out to disk, which can be **less efficient**. Hadoop is an older tech than Spark and one of the cornerstone big data tech.

In this tutorial, it includes a file called `songplay.txt`. This is a text file where each line represent a song that was played in the app Sparkify. The MR code will
count how many times each song was played (**how many times the song title appears in the list**)

## MapReduce vs Hadoop MapReduce

MR is a programming technique, Hadoop MR is a specific implementation of the programming technique.
