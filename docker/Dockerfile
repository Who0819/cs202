FROM ubuntu:jammy

# set environment variables for tzdata
ARG TZ=America/New_York
ENV TZ=${TZ}

# include manual pages and documentation
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update &&\
 yes | unminimize

# install GCC-related packages
RUN apt-get -y install\
 binutils-doc\
 cpp-doc\
 gcc-doc\
 g++\
 g++-multilib\
 gdb\
 gdb-doc\
 glibc-doc\
 libblas-dev\
 liblapack-dev\
 liblapack-doc\
 libstdc++-12-doc\
 make\
 make-doc

# install clang-related packages
RUN apt-get -y install\
 clang\
 clang-14-doc\
 lldb

# install qemu for WeensyOS (sadly, this pulls in a lot of crap)
RUN apt-get -y install\
 qemu-system-x86

# install programs used for system exploration
RUN apt-get -y install\
 blktrace\
 linux-tools-generic\
 strace\
 tcpdump

# install interactive programs (emacs, vim, nano, man, sudo, etc.)
RUN apt-get -y install\
 bc\
 curl\
 dc\
 emacs-nox\
 git\
 git-doc\
 man\
 micro\
 nano\
 psmisc\
 sudo\
 vim\
 wget

# set up libraries
RUN apt-get -y install\
 libreadline-dev\
 locales\
 wamerican

# install programs used for networking
RUN apt-get -y install\
 dnsutils\
 inetutils-ping\
 iproute2\
 net-tools\
 netcat\
 telnet\
 time\
 traceroute

# install infrastructure for grading/testing scripts (cs202)
RUN apt-get -y install\
  python2\ 
  bats

# install ctags for lab1 (cs202)
RUN apt-get -y install\
    exuberant-ctags

# install Posix man pages (cs202)
RUN apt-get -y install\
    manpages-posix\
    manpages-posix-dev

# set up default locale
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8

# remove unneeded .deb files
RUN rm -r /var/lib/apt/lists/*

# set up passwordless sudo for user cs202-user
RUN useradd -m -s /bin/bash cs202-user && \
 echo "cs202-user ALL=(ALL:ALL) NOPASSWD: ALL" > /etc/sudoers.d/cs202-init

# create binary reporting version of dockerfile
RUN (echo '#\!/bin/sh'; echo 'echo 16') > /usr/bin/cs202-docker-version; chmod ugo+rx,u+w,go-w /usr/bin/cs202-docker-version

# git build arguments
ARG USER=CS202\ User
ARG EMAIL=nobody@example.com

# configure your environment
USER cs202-user
RUN git config --global user.name "${USER}" && \
 git config --global user.email "${EMAIL}" && \
 (echo "(custom-set-variables"; echo " '(c-basic-offset 4)"; echo " '(indent-tabs-mode nil))") > ~/.emacs && \
 (echo "set expandtab"; echo "set shiftwidth=4"; echo "set softtabstop=4") > ~/.vimrc && \
 cat /dev/null > ~/.bash_profile && \
 echo "# 2022: avoid a Docker bug with user mapping by listing working directory" >> ~/.bash_profile && \
 echo "ls -al > /dev/null" >> ~/.bash_profile && \
 echo "for i in \`mount | grep /home/cs202-user | sed 's/^.*\\(\\/home[^ ]*\\).*/\\\\1/'\`; do ls -al \$i > /dev/null; done" >> ~/.bash_profile && \
 echo "# make ssh-auth.sock user-readable" >> ~/.bash_profile && \
 (echo "if test -e /run/host-services/ssh-auth.sock; then"; echo "  sudo chown cs202-user:cs202-user /run/host-services/ssh-auth.sock"; echo "fi") >> ~/.bash_profile && \
 echo ". ~/.bashrc" >> ~/.bash_profile && \
 rm -f ~/.bash_logout && \
 echo "add-auto-load-safe-path ~" > ~/.gdbinit

WORKDIR /home/cs202-user
CMD ["/bin/bash", "-l"]

# - This Dockerfile borrowed from Harvard CS 61 (Eddie Kohler, Professor)
# - Initial version of that Dockerfile by Todd Morrill, Harvard CS 61 DCE student
