# Based mostly off of:
# https://github.com/jupyter/docker-stacks/blob/main/images/julia-notebook
ARG REGISTRY=quay.io
ARG OWNER=jupyter
ARG BASE_CONTAINER=$REGISTRY/$OWNER/minimal-notebook
FROM $BASE_CONTAINER

ENV NB_UID=1000
ENV NB_GID=100

# Fix: https://github.com/hadolint/hadolint/wiki/DL4006
# Fix: https://github.com/koalaman/shellcheck/wiki/SC3014
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER "${NB_USER}"

# Install Jupyter Console and Kernel Gateway
RUN pip install jupyter-console jupyter_kernel_gateway

# Install tslab
ENV PATH "$PATH:${CONDA_DIR}/.npm-global/bin"
RUN mkdir "${CONDA_DIR}/.npm-global" \
 && npm config set prefix "${CONDA_DIR}/.npm-global" \
 && npm install -g tslab \
 && tslab install \
 && rm -Rf "${HOME}/.npm-global/bin/tslab"

RUN rm -Rf "${HOME}/.local/share/jupyter/kernels/tslab" \
  && mkdir -p "${CONDA_DIR}/share/jupyter/kernels/tslab"
COPY --chown=${NB_UID}:${NB_GID} "setup-scripts/kernel-tslab.json" "${CONDA_DIR}/share/jupyter/kernels/tslab/kernel.json"
 
# update permissions as root
USER root
RUN fix-permissions /etc/jupyter/ \
 && fix-permissions "${CONDA_DIR}" \
 && fix-permissions "${HOME}"

USER "${NB_USER}"

WORKDIR "${HOME}"

CMD ["jupyter", "kernelgateway", "--KernelGatewayApp.ip=0.0.0.0", "--KernelGatewayApp.port=8888"]
