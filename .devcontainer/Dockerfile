FROM ubuntu:noble

RUN if id "ubuntu" &>/dev/null; then \
            echo "Deleting user 'ubuntu' for noble" && userdel -f -r ubuntu || echo "Failed to delete ubuntu user for noble"; \  
        else \
            echo "User 'ubuntu' does not exist for noble"; \ 
    fi

COPY first-run-notice.txt /tmp/scripts/

ENV LANG="C.UTF-8"

#Merging the mutiple layers to reduce the size of the image slightly
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    # Restore man command
    && yes | unminimize 2>&1 \ 
    # Install basic build tools
    && apt-get upgrade -y \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    #dreamcast
        gawk \
        patch \
        bzip2 \
        tar \
        make \
        libgmp-dev \
        libmpfr-dev \
        libmpc-dev \
        gettext \
        wget \
        libelf-dev \
        texinfo \
        bison \
        flex \
        sed \
        git \
        build-essential \
        diffutils \
        curl \
        libjpeg-dev \
        libpng-dev \
        python3 \
        pkg-config \
        cmake \
        libisofs-dev \
        meson \
        ninja-build \
        rake \
    #ps2 additional
        autopoint \
        autoconf \
        automake \
        libgmp3-dev \
    #psp additional
        libusb-1.0-0-dev \
        libreadline-dev \
        libcurl4 \
        libcurl4-openssl-dev \
        libssl-dev \
        libarchive-dev \
        libgpgme-dev \
        python3-pip \
        python3-venv \
        libncurses-dev \
        pkg-config \
        libtool \
        libz-dev \
    #orbisdev
        findutils \
    #others
        unzip \
        swig3.0 \
        moreutils \
        rsync \
        zip \
        libgdiplus \
        jq \
        # x11 for remote graphic gui
        libx11-dev \ 
        libxft-dev \
        libxext-dev \
        xauth \
    && rm -rf /var/lib/apt/lists/* \
    # This is the folder containing 'links' to benv and build script generator
    && apt-get update \
    && apt-get upgrade -y \
    && add-apt-repository universe \
    && rm -rf /var/lib/apt/lists/* \
    # Verify expected build and debug tools are present
    && apt-get update \
    && apt-get -y install build-essential cmake cppcheck valgrind clang lldb llvm gdb python3-dev \
    # Install tools and shells not in common script
    && apt-get install -yq vim vim-doc xtail software-properties-common libsecret-1-dev \
    # Clean up
    && apt-get autoremove -y && apt-get clean -y \
    # Move first run notice to right spot
    && mkdir -p "/usr/local/etc/vscode-dev-containers/" \
    && mv -f /tmp/scripts/first-run-notice.txt /usr/local/etc/vscode-dev-containers/ \
    # Install and setup fish
    && apt-get install -yq fish \
    && FISH_PROMPT="function fish_prompt\n    set_color green\n    echo -n (whoami)\n    set_color normal\n    echo -n \":\"\n    set_color blue\n    echo -n (pwd)\n    set_color normal\n    echo -n \"> \"\nend\n" \
    && printf "$FISH_PROMPT" >> /etc/fish/functions/fish_prompt.fish \
    && printf "if type code-insiders > /dev/null 2>&1; and not type code > /dev/null 2>&1\n  alias code=code-insiders\nend" >> /etc/fish/conf.d/code_alias.fish \   
    # Remove scripts now that we're done with them
    && apt-get clean -y && rm -rf /tmp/scripts

# Install libssl1.1 for oryx compatibility based on architecture
RUN ARCH=$(uname -m) && \
    if [ "$ARCH" = "x86_64" ] || [ "$ARCH" = "amd64" ]; then \
        curl -fsSL -o libssl1.1_1.1.0g-2ubuntu4_amd64.deb http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.0g-2ubuntu4_amd64.deb && \
        dpkg -i libssl1.1_1.1.0g-2ubuntu4_amd64.deb && \
        rm libssl1.1_1.1.0g-2ubuntu4_amd64.deb; \
    elif [ "$ARCH" = "aarch64" ] || [ "$ARCH" = "arm64" ]; then \
        curl -fsSL -o libssl1.1_1.1.1f-1ubuntu2_arm64.deb http://ports.ubuntu.com/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2_arm64.deb && \
        dpkg -i libssl1.1_1.1.1f-1ubuntu2_arm64.deb && \
        rm libssl1.1_1.1.1f-1ubuntu2_arm64.deb; \
    else \
        echo "Unsupported architecture: $ARCH" && exit 1; \
    fi

# Default to bash shell (other shells available at /usr/bin/fish and /usr/bin/zsh)
ENV SHELL=/bin/bash \
    DOCKER_BUILDKIT=1

# Mount for docker-in-docker 
#VOLUME [ "/var/lib/docker" ]

#CMD [ "sleep", "infinity" ]

# [Optional] Install debugger for development of Codespaces - Not in resulting image by default
ARG DeveloperBuild
RUN if [ -z $DeveloperBuild ]; then \
        echo "not including debugger" ; \
    else \
        curl -sSL https://aka.ms/getvsdbgsh | bash /dev/stdin -v latest -l /vsdbg ; \
    fi
