FROM continuumio/miniconda3:latest

RUN apt-get update && apt-get install -y \
        protobuf-compiler \
        sudo \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

COPY environment.yml /tmp/
RUN conda env create -f /tmp/environment.yml \
    && conda clean -afy \
    && find /opt/conda/ -follow -type f -name '*.a' -delete \
    && find /opt/conda/ -follow -type f -name '*.pyc' -delete \
    && find /opt/conda/ -follow -type f -name '*.js.map' -delete \
    && rm /tmp/environment.yml

ARG username
ARG userid

ARG home=/home/${username}
ARG workdir=${home}/loan_default_project

RUN adduser ${username} --uid ${userid} --gecos '' --disabled-password \
    && echo "${username} ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/${username} \
    && chmod 0440 /etc/sudoers.d/${username}

WORKDIR ${workdir}
RUN chown ${username}:${username} ${workdir}

USER ${username}
WORKDIR ${workdir}

ENV PATH /opt/conda/envs/ml_env/bin:$PATH

# The config below enables diffing notebooks with nbdiff (and nbdiff support
# in git diff command) after connecting to the container by "make exec" (or
# "docker-compose exec loan_default_project bash")
#       You may also try running:
#         nbdiff NOTEBOOK_NAME.ipynb
#       to get nbdiff between checkpointed version and current version of the
# given notebook.

RUN git-nbdiffdriver config --enable --global

# INFO: Optionally uncomment any (one) of the following RUN commands below to ignore either
#       metadata or details in nbdiff within git diff
#RUN git config --global diff.jupyternotebook.command 'git-nbdiffdriver diff --ignore-metadata'
RUN git config --global diff.jupyternotebook.command 'git-nbdiffdriver diff --ignore-details'


COPY docker/bashrc.bash /tmp/
RUN cat /tmp/bashrc.bash >> ${home}/.bashrc \
    && echo "export PATH=\"${workdir}/docker/bin:$PATH\"" >> ${home}/.bashrc \
    && sudo rm /tmp/bashrc.bash
