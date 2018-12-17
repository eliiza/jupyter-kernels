#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

FROM openjdk:8-alpine

ARG spark_version=spark-2.4.0-bin-hadoop2.7
ARG img_path=kubernetes/dockerfiles
ARG k8s_tests=kubernetes/tests

RUN set -ex && \
    apk upgrade --no-cache && \
    apk add --no-cache bash tini libc6-compat linux-pam && \
    mkdir -p /opt/spark && \
    mkdir -p /opt/spark/work-dir && \
    touch /opt/spark/RELEASE && \
    rm /bin/sh && \
    ln -sv /bin/bash /bin/sh && \
    echo "auth required pam_wheel.so use_uid" >> /etc/pam.d/su && \
    chgrp root /etc/passwd && chmod ug+rw /etc/passwd

WORKDIR /
RUN wget https://www-us.apache.org/dist/spark/spark-2.4.0/${spark_version}.tgz && \
  ls -la . && \
  tar -vxzf ${spark_version}.tgz && \
  mv ${spark_version}/jars /opt/spark/jars && \
  mv ${spark_version}/bin /opt/spark/bin && \
  mv ${spark_version}/sbin /opt/spark/sbin && \
  mv ${spark_version}/examples /opt/spark/examples && \
  mv ${spark_version}/data /opt/spark/data && \
  mv ${spark_version}/${img_path}/spark/entrypoint.sh /opt/ && \
  mv ${spark_version}/${k8s_tests} /opt/spark/tests && \
  rm -Rf ${spark_version}.tgz ${spark_version}

# Add S3 Support
ARG AWS_SDK_VERSION="1.7.4"
ARG AWS_HADOOP_VERSION="2.7.4"
RUN mkdir -p /tmp/jars && \
    wget http://central.maven.org/maven2/com/amazonaws/aws-java-sdk/${AWS_SDK_VERSION}/aws-java-sdk-${AWS_SDK_VERSION}.jar -O /opt/spark/jars/aws-java-sdk-${AWS_SDK_VERSION}.jar && \
    wget http://central.maven.org/maven2/org/apache/hadoop/hadoop-aws/${AWS_HADOOP_VERSION}/hadoop-aws-${AWS_HADOOP_VERSION}.jar -O /opt/spark/jars/hadoop-aws-${AWS_HADOOP_VERSION}.jar


ENV SPARK_HOME /opt/spark

WORKDIR /opt/spark/work-dir

ENTRYPOINT [ "/opt/entrypoint.sh" ]
