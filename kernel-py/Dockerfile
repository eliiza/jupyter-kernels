# Ubuntu 18.04.1 LTS Bionic
FROM jupyter/scipy-notebook:latest

COPY kernel-launchers /usr/local/share/jupyter/kernel-launchers/
COPY kernel-launchers/bootstrap/bootstrap-kernel.sh /usr/local/bin/

COPY common-python/requirements.txt /tmp/requirements.txt
RUN conda install --yes --file /tmp/requirements.txt

USER root

RUN chown -R jovyan:users /usr/local/share/jupyter

USER jovyan
