---
title: How to build Wayang
sidebar_position: 1
id: installation
---

<!--

  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.

-->
# Installing and building Apache Wayang

## Clone repository
```shell
git clone https://github.com/apache/incubator-wayang.git
```

## Create binaries
Running following commands to build Wayang and generate the tar.gz
```shell
cd incubator-wayang
./mvnw clean install -DskipTests
./mvnw clean package -pl :wayang-assembly -Pdistribution
```
Then you can find the `wayang-assembly-0.7.1-SNAPSHOT-dist.tar.gz` under `wayang-assembly/target` directory.


## Prepare the environment
### Wayang
```shell
tar -xvf wayang-assembly-0.7.1-SNAPSHOT-dist.tar.gz
cd wayang-0.7.1-SNAPSHOT
```

In linux
```shell
echo "export WAYANG_HOME=$(pwd)" >> ~/.bashrc
echo "export PATH=${PATH}:${WAYANG_HOME}/bin" >> ~/.bashrc
source ~/.bashrc
```
In MacOS
```shell
echo "export WAYANG_HOME=$(pwd)" >> ~/.zshrc
echo "export PATH=${PATH}:${WAYANG_HOME}/bin" >> ~/.zshrc
source ~/.zshrc
```
### Others
- You need to install Apache Spark version 3 or higher. Don’t forget to set the `SPARK_HOME` environment variable.
- You need to install Apache Hadoop version 3 or higher. Don’t forget to set the `HADOOP_HOME` environment variable.

## Run the program

To execute the WordCount example with Apache Wayang, you need to execute your program with the 'wayang-submit' command:

```shell
cd wayang-0.7.1-SNAPSHOT
./bin/wayang-submit org.apache.wayang.apps.wordcount.Main java file://$(pwd)/README.md
```
Then you should be able to see the output of the Wordcount example.

# Compiling Apache Wayang

Apache Wayang (incubating) has different dependencies, for compiling, it needs to add some profile in the compilation to enable maven works properly.

 ```shell
mvn clean compile
```

The line before is because the plugin the Antlr is not needed in all the modules, as well it has happened with Scala language.

When maven compiles one or more modules using those plugins in the compilation time, it needs to add.

The modules are:
- wayang-api-scala-java
- wayang-core (Antlr)
- wayang-iejoin
- wayang-spark
- wayang-profiler
- wayang-tests-integration

# Executing Coverage

```shell
mvn clean verify jacoco:report
```

the final report is placed on `./target/aggregate.exec/aggregate.exec`
