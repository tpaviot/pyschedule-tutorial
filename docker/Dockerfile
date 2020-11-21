FROM continuumio/miniconda3:4.8.2

COPY environment.yml labextensions.txt /tmp/

RUN adduser --disabled-password --gecos "Default user" --uid 1000 teacher && \
    apt-get update -y && \
    apt-get install --no-install-recommends -y libgl1-mesa-glx libglu1-mesa && \
    apt-get clean

RUN conda env create -f /tmp/environment.yml -n teacher && \
    conda install -n teacher -y nodejs && \
    conda run -n teacher jupyter labextension install --no-build $(cat /tmp/labextensions.txt) && \
    conda run -n teacher jupyter lab build --dev-build=True --minimize=False && \
    conda clean --all && \
    conda run -n teacher jupyter lab clean && \
    find / -type f -name '*.py[co]' -delete -o -type d -name __pycache__ -delete

VOLUME /home/teacher/
WORKDIR /home/teacher
EXPOSE 8888

ADD --chown=teacher:teacher examples /home/teacher

USER teacher

CMD bash -l -c "source /opt/conda/bin/activate cq && jupyter lab --ip=0.0.0.0 --no-browser --NotebookApp.token='' --NotebookApp.allow_origin='*'"
