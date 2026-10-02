# Start with BioSim base image.
ARG BASE_IMAGE=latest@sha256:9396d59c0b195cb365875343f26a67ef27a6a6260d32be3fcb29323f8bb0ca05
FROM ghcr.io/ccpbiosim/jupyterhub-base:$BASE_IMAGE

LABEL maintainer="James Gebbie-Rayet <james.gebbie@stfc.ac.uk>"
ARG TARGETPLATFORM

USER $NB_USER
WORKDIR $HOME

# Dependencies for the workshop
RUN conda install -y -c conda-forge -c ccpbiosim \
    conda-forge::aiida-core=2.9.2 \
    ccpbiosim::aiida-gromacs=2.3.1 \
    conda-forge::gromacs=2026.3=nompi_*

RUN git clone https://github.com/PSDI-UK/aiida-gromacs.git && \
    mv aiida-gromacs/examples/lysozyme_files . && \
    mv aiida-gromacs/notebooks/lysozyme_tutorial.ipynb . && \
    rm -rf aiida-gromacs

COPY --chown=1000:100 docker/default-37a8.jupyterlab-workspace /home/jovyan/.jupyter/lab/workspaces/default-37a8.jupyterlab-workspace

# Always finish with non-root user as a precaution.
USER $NB_USER
