# You can pick any Debian/Ubuntu-based image. 😊
FROM mcr.microsoft.com/vscode/devcontainers/base:buster

COPY library-scripts/*.sh /tmp/library-scripts/

# [Option] Install zsh
ARG INSTALL_ZSH="true"
# [Option] Upgrade OS packages to their latest versions
ARG UPGRADE_PACKAGES="false"

# Install needed packages and setup non-root user. Use a separate RUN statement to add your own dependencies.
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && bash /tmp/library-scripts/common-debian.sh "${INSTALL_ZSH}" "${USERNAME}" "${USER_UID}" "${USER_GID}" "${UPGRADE_PACKAGES}" \
    && apt-get install -y libssl-dev libffi-dev python3-dev python3-pip \
    && apt-get clean -y && rm -rf /var/lib/apt/lists/*

# Install Ansible
RUN pip3 install --upgrade pip setuptools
RUN pip3 install ansible==2.10.4 pyvmomi pywinrm ansible-tower-cli git+https://github.com/vmware/vsphere-automation-sdk-python.git 
## still need to install this... 
ENTRYPOINT [ "/usr/local/share/docker-init.sh" ]
CMD [ "sleep", "infinity" ]

#[Optional] Uncomment this section to install additional OS packages.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends  ripgrep software-properties-common sshpass
RUN ansible-galaxy collection install community.vmware

## packer
RUN /tmp/library-scripts/packer.sh
