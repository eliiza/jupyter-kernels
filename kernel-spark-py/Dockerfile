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

USER root
WORKDIR /
# TODO: Investigate running both pip and pip3 via virtualenvs
RUN apk add --no-cache python && \
    apk add --no-cache python3 && \
    python -m ensurepip && \
    python3 -m ensurepip && \
    # We remove ensurepip since it adds no functionality since pip is
    # installed on the image and it just takes up 1.6MB on the image
    rm -r /usr/lib/python*/ensurepip && \
    pip install --upgrade pip setuptools && \
    # You may install with python3 packages by using pip3.6
    # Removed the .cache to save space
    rm -r /root/.cache

WORKDIR /opt/spark/work-dir
ENTRYPOINT [ "/opt/entrypoint.sh" ]

### kernel
RUN apk add --no-cache build-base libffi-dev openssl-dev python-dev && \
    pip install cffi ipykernel ipython jupyter_client pycrypto

COPY common-python/requirements.txt /tmp/requirements.txt
RUN conda install --yes --file /tmp/requirements.txt

COPY kernel-launchers /usr/local/share/jupyter/kernel-launchers/
RUN adduser -S -u 1000 -G users jovyan && \
    chown -R jovyan:users /usr/local/share/jupyter /opt/spark/work-dir

USER jovyan
