javac -classpath /opt/hadoop-core-1.2.1.jar:/opt/commons-cli-1.2.jar -d /usr/local/examples/word\_count\_class WordCount.java

jar -cvf wordcount.jar \*.class added manifest

hadoop fs -ls output\_wordcount

 hadoop fs -cat output\_wordcount/part-r-00000

