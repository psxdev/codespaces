ARG VARIANT="noble"
FROM ubuntu:noble
#buildpack-deps:${VARIANT}-curl
#LABEL dev.containers.features="common"

ARG VARIANT
RUN if [ "$VARIANT" = "noble" ]; then \
        if id "ubuntu" &>/dev/null; then \
            echo "Deleting user 'ubuntu'  for $VARIANT" && userdel -f -r ubuntu || echo "Failed to delete ubuntu user for $VARIANT"; \  
        else \
            echo "User 'ubuntu' does not exist for $VARIANT"; \ 
        fi; \
    fi

COPY first-run-notice.txt /tmp/scripts/

# [Optional] Uncomment this section to install additional OS packages.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && yes | unminimize 2>&1 \
    && apt-get -y install --no-install-recommends \
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
        git \
        git-lfs \
        unzip \
        swig3.0 \
        moreutils \
        rsync \
        zip \
        libgdiplus \
        jq \
        cppcheck \
        valgrind \
        clang \
        lldb \
        llvm \
        gdb \
        python3-dev \
        vim \
        vim-doc \
        xtail \
        software-properties-common \
        libsecret-1-dev \
# x11 for remote graphic gui and raylib for linux
        libx11-dev \ 
        libxft-dev \
        libxext-dev \
        xauth \ 
        x11-apps \
        libx11-dev \
        libxrandr-dev \
        libxi-dev \
        mesa-utils \
        libgl1-mesa-dev \
        libglu1-mesa-dev \
        libxcursor-dev \
        libxinerama-dev \
        libwayland-dev \
        libxkbcommon-dev \
        libasound2-dev \
#upgrade and add repository
    && apt-get upgrade -y \
    && add-apt-repository universe \
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