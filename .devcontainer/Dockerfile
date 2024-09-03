FROM mcr.microsoft.com/devcontainers/miniconda:1-3

# install jupyterlab
ENV CONDA_ENVS_DIRS /work/envs
ENV SHELL=/bin/bash
RUN conda install -y jupyterlab && \
    jupyter labextension disable "@jupyterlab/apputils-extension:announcements" && \
    ln -s /opt/conda/bin/jupyter-lab /opt/conda/bin/jupyter-notebook

# Copy environment.yml (if found) to a temp location so we update the environment. Also
# copy "noop.txt" so the COPY instruction does not fail if no environment.yml exists.
COPY environment.yml* .devcontainer/noop.txt /tmp/conda-tmp/

USER 1000
RUN if [ -f "/tmp/conda-tmp/environment.yml" ]; then umask 0002 && \
    /opt/conda/bin/conda env create -n project -f /tmp/conda-tmp/environment.yml; fi && \
    conda init && \
    conda install -y -n project ipykernel && \
    /opt/conda/envs/project/bin/python -m ipykernel install --name project --display-name "Project environment" --user && \
    echo "conda activate project" >> $HOME/.bashrc
