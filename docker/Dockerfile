# Start with BioSim base image.
ARG BASE_IMAGE=latest
FROM ghcr.io/ccpbiosim/jupyterhub-base:$BASE_IMAGE

LABEL maintainer="James Gebbie-Rayet <james.gebbie@stfc.ac.uk>"
LABEL org.opencontainers.image.source=https://github.com/ccpbiosim/aiida-lysozyme-workshop
LABEL org.opencontainers.image.description="A container environment for the PSDI workshop on AiiDA tools for data collection."
LABEL org.opencontainers.image.licenses=MIT

ARG TARGETPLATFORM

USER root
WORKDIR /opt/

# Install RabbitMQ for Mac
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends \
    erlang  && \
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    wget -c --no-check-certificate https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.10.14/rabbitmq-server-generic-unix-3.10.14.tar.xz && \
    tar -xf rabbitmq-server-generic-unix-3.10.14.tar.xz && \
    rm rabbitmq-server-generic-unix-3.10.14.tar.xz && \
    ln -sf /opt/rabbitmq_server-3.10.14/sbin/* /usr/local/bin/ && \
    fix-permissions /opt/rabbitmq_server-3.10.14

USER $NB_USER
WORKDIR $HOME

# Dependencies for the workshop
RUN conda install mamba
RUN mamba install -c conda-forge -y aiida-core
RUN if [ "$TARGETPLATFORM" = "linux/amd64" ]; then \
      conda install conda-forge/linux-64::gromacs=2024.5=nompi_h5f56185_100 -y; \
    elif [ "$TARGETPLATFORM" = "linux/arm64" ]; then \
      conda install conda-forge/linux-aarch64::gromacs=2024.5=nompi_h9afd374_100 -y; \
    fi
RUN pip3 install aiida-gromacs

RUN git clone https://github.com/PSDI-UK/aiida-gromacs.git && \
    mv aiida-gromacs/examples/lysozyme_files . && \
    mv aiida-gromacs/notebooks/lysozyme_tutorial.ipynb . && \
    rm -rf aiida-gromacs

RUN echo "./aiida-start" >> ~/.bashrc
COPY --chown=1000:100 docker/default-37a8.jupyterlab-workspace /home/jovyan/.jupyter/lab/workspaces/default-37a8.jupyterlab-workspace
COPY --chown=1000:100 aiida-start /home/jovyan/
COPY --chown=1000:100 aiida-stop /home/jovyan/

# UNCOMMENT THIS LINE FOR REMOTE DEPLOYMENT
COPY docker/jupyter_notebook_config.py /etc/jupyter/

# Always finish with non-root user as a precaution.
USER $NB_USER
