#Frequently asked questions
##How is the memory consumption?
At present, when us3vmds tests save more than 100W file indexes, the memory is about 1.7g, which contains many materialized views of directory serialization, and the theoretical and actual values are relatively less. The average size of each index is about 1.7k, which is indeed relatively high, which is also one of the subsequent optimization points.
##After the us3vmds is started, it is found that the files in the directory seen through the Hadoop command are inconsistent with those seen on the US3 console. What should I do?
There are two methods. The first is to restart us3vmds violently, and the second is to force synchronization through the sync command of us3vmds.