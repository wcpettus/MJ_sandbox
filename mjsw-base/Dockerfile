FROM python:3.9.13-slim-bullseye
LABEL maintainer "wcpettus@gmail.com"

# System packages as a base layer
# - line 1 are ROOT required packages
# - lines 2-4 are ROOT optional packages
# - line 5 are other dependencies (GAT needs curl) and quality-of-life
RUN apt-get -y update && apt-get -y install \
  dpkg-dev cmake g++ gcc binutils libx11-dev libxpm-dev libxft-dev libxext-dev libssl-dev \
  gfortran libpcre3-dev xlibmesa-glu-dev libglew1.5-dev libftgl-dev libmariadb-dev \
  libfftw3-dev libcfitsio-dev graphviz-dev libavahi-compat-libdnssd-dev libldap2-dev \
  python-dev libxml2-dev libkrb5-dev libgsl0-dev qtwebengine5-dev \
  vim git wget curl libcurl4-openssl-dev\
  && apt-get autoremove -y && rm -rf /var/lib/apt/lists*

# Stuck on 6-26-00 as of 1 June 2022 - -04 has a broken xrootd hash
ENV ROOT_VERSION 6-26-00
ENV ROOTSYS /usr/local/root
ENV PATH $ROOTSYS/bin:$PATH
ENV PYTHONDIR $PYTHONDIR:$ROOTSYS
ENV LD_LIBRARY_PATH $ROOTSYS/lib:$PYTHONDIR/lib:$ROOTSYS/bindings/pyroot:$LD_LIBRARY_PATH
ENV DYLD_LIBRARY_PATH /usr/lib:$ROOTSYS/lib:$PYTHONDIR/lib:$ROOTSYS/bindings/pyroot:$DYLD_LIBRARY_PATH
ENV PYTHONPATH $PYTHONPATH:$ROOTSYS/lib:$ROOTSYS/bindings/pyroot
RUN mkdir -p /mjsw/ROOT /mjsw/ROOT/build
WORKDIR /mjsw/ROOT
RUN wget --quiet https://github.com/root-project/root/archive/v${ROOT_VERSION}.tar.gz \
  && tar -xf v${ROOT_VERSION}.tar.gz \
  && mv root-${ROOT_VERSION} root \
  && rm -rf v${ROOT_VERSION}.tar.gz \
  && cd build \
  && cmake ../root -Dgdml=ON -Dminuit2=ON -Dhttp=ON -Dldap=OFF -Dfortran=OFF \
    -DCMAKE_CXX_FLAGS=-D_GLIBCXX_USE_CXX11_ABI=0 \
    -DCMAKE_INSTALL_PREFIX:PATH=/usr/local/root \
  && make -j$(nproc); make && make install \
  && rm -rf /mjsw/ROOT/*

# Current (2.4.5.2) breaks GAT build due to new units, expect 2.4.4.1 to be safe
ENV CLHEP_VERSION 2.4.4.1
ENV CLHEP_BASE_DIR /mjsw/CLHEP/$CLHEP_VERSION/install
ENV LD_LIBRARY_PATH $CLHEP_BASE_DIR/lib:$LD_LIBRARY_PATH
RUN mkdir -p /mjsw/CLHEP/$CLHEP_VERSION/install /mjsw/CLHEP/$CLHEP_VERSION/build
WORKDIR /mjsw/CLHEP/$CLHEP_VERSION/build
RUN wget -P /mjsw/CLHEP https://proj-clhep.web.cern.ch/proj-clhep/dist1/clhep-$CLHEP_VERSION.tgz \
  && tar zxvf /mjsw/CLHEP/clhep-$CLHEP_VERSION.tgz -C /mjsw/CLHEP/ \
  && cmake -DCMAKE_INSTALL_PREFIX=../install ../CLHEP \
  && cmake --build . --config RelWithDebInfo \
  && cmake --build . --target install \
  && rm -rf * \
  && cd /mjsw/CLHEP/$CLHEP_VERSION \
  && rmdir build \
  && rm /mjsw/CLHEP/clhep-$CLHEP_VERSION.tgz

RUN pip3 install --upgrade pip && pip3 install numpy matplotlib pandas notebook
