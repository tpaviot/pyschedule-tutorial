FROM continuumio/miniconda3:4.8.2

# see https://towardsdatascience.com/conda-pip-and-docker-ftw-d64fe638dc45
# Create a non-root user
RUN adduser --disabled-password --gecos "Default user" --uid 1000 student

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

VOLUME /home/student/
WORKDIR /home/student
EXPOSE 8888

CMD bash -l -c "source /opt/conda/bin/activate pyschedule-tuto && jupyter lab --ip=0.0.0.0 --no-browser --NotebookApp.token='' --NotebookApp.allow_origin='*'"
