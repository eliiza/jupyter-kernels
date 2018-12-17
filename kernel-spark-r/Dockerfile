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

FROM eliiza/spark-base:latest
WORKDIR /
RUN apk add --no-cache R R-dev
ENV R_HOME /usr/lib/R
WORKDIR /opt/spark/work-dir
ENTRYPOINT [ "/opt/entrypoint.sh" ]


### Kernel

# Install items necessary for IRKernel and launcher to run.
# First some python things...
RUN apk add --no-cache build-base libzmq python-dev && \
    python -m ensurepip && \
    rm -r /usr/lib/python*/ensurepip && \
    pip install --upgrade pip setuptools jupyter_client && \
    rm -r /root/.cache

# then some R things...
COPY common-r/install.R /tmp/install.R
RUN mkdir -p /usr/share/doc/R/html && \
    Rscript --slave --no-save --no-restore-history /tmp/install.R && \
    rm /tmp/install.R

# Install OOTB kernelspecs
COPY kernel-launchers /usr/local/share/jupyter/kernel-launchers/

RUN adduser -S -u 1000 -G users jovyan && \
	chown -R jovyan:users \
    /usr/local/share/jupyter \
    /opt/spark/work-dir \
    ${R_HOME}/library

USER jovyan

ENV R_LIBS_USER ${R_HOME}/library:${SPARK_HOME}/R/lib
