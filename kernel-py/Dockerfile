# Ubuntu 18.04.1 LTS Bionic
FROM jupyter/scipy-notebook:61d8aaedaeaf

COPY kernel-launchers /usr/local/share/jupyter/kernel-launchers/

COPY common-python/requirements.txt /tmp/requirements.txt
RUN pip install -r /tmp/requirements.txt && rm -f /tmp/requirements.txt

USER root

RUN adduser -S -u 1000 -G users jovyan && \
    chown -R jovyan:users /usr/local/share/jupyter /opt/spark/work-dir

USER jovyan
