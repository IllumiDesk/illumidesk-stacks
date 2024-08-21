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

# Install deno
RUN curl -fsSL https://deno.land/x/install/install.sh | sh \
 && "${HOME}/.deno/bin/deno" jupyter --unstable --install \
 && mv "${HOME}/.deno/bin/deno" "${CONDA_DIR}/bin" \
 && rm -Rf "${HOME}/.deno/bin/deno"

RUN rm -Rf "${HOME}/.local/share/jupyter/kernels/deno" \
 && mkdir -p "${CONDA_DIR}/share/jupyter/kernels/deno"
COPY --chown=${NB_UID}:${NB_GID} "setup-scripts/kernel-deno.json" "${CONDA_DIR}/share/jupyter/kernels/deno/kernel.json"

# update permissions as root
USER root
RUN fix-permissions /etc/jupyter/ \
 && fix-permissions "${CONDA_DIR}" \
 && fix-permissions "${HOME}"

USER "${NB_USER}"

WORKDIR "${HOME}"

CMD ["jupyter", "kernelgateway", "--KernelGatewayApp.ip=0.0.0.0", "--KernelGatewayApp.port=8888"]
