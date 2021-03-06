FROM yhavinga/nvidia-pytorch-scipy-jupyter:pytorch1.0.0-cuda10.0-py3.6-ubuntu18.04

USER $NB_USER

#---------------- Final add-ons -------------
# ggplot
# Pixiedust debugger
# PostgreSQL client
# Tensorboard
# Python-markdown in notebooks
# nbstripout to prevent cell output in git commits
# and some other add ons
#
# Pinning python to 3.6 is required to solve ggplot and pandas dependencies.
#--------------------------------------------
RUN conda install --quiet --yes python=3.6 \
        'biobuilds::postgresql-client' \
        nodejs \
        Cython \
        tensorflow \
	'conda-forge::ggplot' \
        psycopg2 && \
    conda install -c conda-forge \
        ipython-sql \
        tensorboardx \
        nbstripout \
        jupyter_contrib_nbextensions \
        jupyter_nbextensions_configurator \
        jupyterlab-plotly-extension \
        hide_code && \
    conda clean --all -f -y && \
    jupyter nbextension enable python-markdown/main --sys-prefix && \
    jupyter nbextension enable hide_code/hide_code --sys-prefix && \
    jupyter labextension install @jupyterlab/git && \
    jupyter labextension install jupyterlab_tensorboard && \
    pip install jupyter_tensorboard \
	jupyterlab-git \
	pixiedust \
        torchvision \
        scikit-image  \
        h5py-cache \
        torchnet \
	pixiedust \
        graphviz && \
    jupyter serverextension enable --py jupyterlab_git && \
    jupyter labextension install @jupyterlab/toc && \
    jupyter labextension install jupyterlab-drawio && \
    npm cache clean --force && \
    rm -rf $CONDA_DIR/share/jupyter/lab/staging && \
    rm -rf /home/$NB_USER/.cache/yarn && \
    rm -rf /home/$NB_USER/.node-gyp && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

USER root

RUN git config --global core.excludesfile ~/.gitignore_global && \
    echo "**/*.ipynb_checkpoints/" >> ~/.gitignore_global && \
    git config --global core.attributesfile ~/.gitattributes_global && \
    git config --global filter.nbstripout.clean '$(which nbstripout)' && \
    git config --global filter.nbstripout.smudge cat && \
    git config --global filter.nbstripout.required true && \
    echo "*.ipynb filter=nbstripout" >> ~/.gitattributes_global && \
    echo "*.ipynb diff=ipynb" >> ~/.gitattributes_global && \
    git config --global diff.ipynb.textconv '$(which nbstripout) -t' && \
    git config --global user.email "you@example.com" && \
    git config --global user.name "Your Name"

# Fix permissions on /etc/jupyter as root
RUN fix-permissions /etc/jupyter/

# Switch back to jovyan to avoid accidental container runs as root
USER $NB_UID
