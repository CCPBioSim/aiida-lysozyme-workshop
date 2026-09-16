# Start with BioSim base image.
ARG BASE_IMAGE=latest@sha256:5eb67b09e3fdf64b3128bd9501e08a0cbb007804b678f2096728d8c1260e6217
FROM ghcr.io/ccpbiosim/jupyterhub-base:$BASE_IMAGE

LABEL maintainer="James Gebbie-Rayet <james.gebbie@stfc.ac.uk>"
ARG TARGETPLATFORM

USER $NB_USER
WORKDIR $HOME

# Dependencies for the workshop
RUN conda install -y -c conda-forge -c ccpbiosim \
    conda-forge::aiida-core=2.9.2 \
    ccpbiosim::aiida-gromacs=2.2.2 \
    conda-forge::gromacs=2026.3=nompi_*

RUN git clone https://github.com/PSDI-UK/aiida-gromacs.git && \
    mv aiida-gromacs/examples/lysozyme_files . && \
    mv aiida-gromacs/notebooks/lysozyme_tutorial.ipynb . && \
    rm -rf aiida-gromacs

RUN echo "./aiida-start" >> ~/.bashrc
COPY --chown=1000:100 docker/default-37a8.jupyterlab-workspace /home/jovyan/.jupyter/lab/workspaces/default-37a8.jupyterlab-workspace

# Always finish with non-root user as a precaution.
USER $NB_USER
