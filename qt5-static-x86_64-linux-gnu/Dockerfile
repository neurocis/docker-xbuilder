FROM neurocis/xbuilder
MAINTAINER Leigh Phillips <neurocis@neurocis.me>

# Install dependencies
RUN apt-key update && apt-get update                    \
 && apt-get install -y -q                               \
        perl                                            \
        python                                          \
        libgl1-mesa-dev                                 \
        build-essential                                 \
        git-core                                        \
 && apt-get clean

ENV CROSS_TRIPLE=x86_64-linux-gnu

# Download qt5-source
RUN mkdir -p /opt/qt5-source                            \
 && cd /opt/qt5-source                                  \
 && git clone https://git.gitorious.org/qt/qt5.git .

# Initialize repo & download modules
RUN cd /opt/qt5-source                                  \
 && git checkout 5.5                                    \
 && perl init-repository --no-webkit

RUN cp -R /opt/qt5-source/ /opt/qt5-static-${CROSS_TRIPLE} \
 && ln -sfnv /opt/qt5-static-${CROSS_TRIPLE} /opt/qt5

RUN cd /opt/qt5                                         \
 && ./configure -prefix /opt/qt5/                       \
                -static                                 \
                -opensource                             \
                -confirm-license                        \
                -developer-build                        \
                -qt-xcb                                 \
                -nomake examples                        \
                -nomake tests                           \
                -skip qtwebkit

RUN cd /opt/qt5                                         \
 && xbuild make -j$(nproc) all

