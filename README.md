#Demo Git Repository

This is the first file in this repo.

##Spark streaming code Below
import org.apache.spark._
import org.apache.spark.streaming._
sc.setLogLevel(ERROR)
// Create a local StreamingContext with batch interval of 10 second
val ssc = new StreamingContext(sc, Seconds(2))
// Create a DStream that will connect to hostname:port, like localhost:9999
val lines = ssc.socketTextStream("localhost", 9999)
// Split each line in each batch into words
val words = lines.flatMap(_.split(" "))
// Count each word in each batch
val pairs = words.map(word => (word, 1))
val wordCounts = pairs.reduceByKey(_ + _)
// Print the elements of each RDD generated in this DStream to the console
wordCounts.print()
// Start the computation
ssc.start()
// Wait for the computation to terminate
ssc.awaitTermination()

start server using telnet 	nc -lk 9999

using spark submit - spark-submit --class "WordCount" --master "local[2]" target/scala-2.10/word-count_2.10-1.0.jar