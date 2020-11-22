FROM continuumio/miniconda3:4.8.2

COPY environment.yml /tmp/

RUN conda env create -f /tmp/environment.yml -n pyschedule-tuto

# Make RUN commands use the new environment:
SHELL ["conda", "run", "-n", "pyschedule-tuto", "/bin/bash", "-c"]

#
# Install pyschedule personal fork
#

WORKDIR /opt/build
RUN git clone https://github.com/tpaviot/pyschedule forkpyschedule
WORKDIR /opt/build/forkpyschedule
RUN git checkout tut-pyschedule
WORKDIR /opt/build/forkpyschedule/src
RUN python setup.py build
RUN python setup.py install

WORKDIR /home/jovyan

USER jovyan

CMD bash -l -c "source /opt/conda/bin/activate pyschedule-tuto"
