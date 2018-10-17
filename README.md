senti
==========================

* Retrieve datasets using Spark Streaming
* Language detection
* Sentiment Analysis (StanfordNLP)   
* Index tweets in Elasticsearch 
* Live dashboard using Kibana


### Docker setup

Configure Twitter OAuth in [`docker-compose.yml`](./docker-compose.yml) file.

After installing docker & docker-compose, you can launch the stack with the following commands:

    docker-compose build
    docker-compose up


After a while, Elasticsearch, Kibana, and Spark should be up:

 * http://0.0.0.0:4040/streaming
 * http://0.0.0.0:9200/_plugin/head
 * http://0.0.0.0:5601 


### Manual setup

    curl -O http://d3kbcqa49mib13.cloudfront.net/spark-1.5.2.tgz
    tar xvzf spark-1.5.2.tgz
    cd spark-1.5.2
    ./sbt/sbt assembly
    
    cd ..

    curl -O https://download.elasticsearch.org/elasticsearch/release/org/elasticsearch/distribution/tar/elasticsearch/2.1.1/elasticsearch-2.1.1.tar.gz
    tar xvzf elasticsearch-2.1.1.tar.gz
    cd elasticsearch-2.1.1
    bin/plugin -install mobz/elasticsearch-head
    bin/elasticsearch -d

    cd ..
    chmod a+x insert.dashboard.sh
    ./insert.dashboard.sh

    curl -O https://download.elastic.co/kibana/kibana/kibana-4.3.1-linux-x64.tar.gz
    tar xvzf kibana-4.3.1-linux-x64.tar.gz 
    cd kibana-4.3.1-linux-x64
    bin/kibana

    cd ..

    curl -L -O https://dl.bintray.com/sbt/native-packages/sbt/0.13.9/sbt-0.13.9.tgz
    tar xvzf sbt-0.13.9.tgz

    JAVA_OPTS=-Xmx2G sbt/bin/sbt assembly

    ../spark-1.5.2/bin/spark-submit \
    --class com.github.shobhitsharma.TwitterSentimentAnalysis \
    --master local[2] \
    target/scala-2.10/twitter-sentiment-analysis-assembly-0.1-SNAPSHOT.jar \
    <consumer_key> \
    <consumer_secret> \
    <access_token> \
    <access_token_secret> \
    [<filters>]
