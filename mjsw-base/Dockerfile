FROM python:3.6.4-jessie
LABEL maintainer "wcpettus@gmail.com"
ENV TERM xterm

# Update cmake to >3.2 (CMake executables will be in /opt/cmake-3.5.2-Linux-x86_64/bin/.)
RUN curl -sSL https://cmake.org/files/v3.5/cmake-3.5.2-Linux-x86_64.tar.gz | tar -xzC /opt
ENV PATH /opt/cmake-3.5.2-Linux-x86_64/bin/:$PATH

RUN apt-get -y update && apt-get -y install \
  git dpkg-dev g++ gcc binutils libx11-dev libxpm-dev libxft-dev libxext-dev \
  gfortran libssl-dev libpcre3-dev xlibmesa-glu-dev libglew1.5-dev libftgl-dev \
  libmysqlclient-dev libfftw3-dev libcfitsio-dev graphviz-dev libavahi-compat-libdnssd-dev \
  libldap2-dev python-dev libxml2-dev libkrb5-dev libgsl0-dev libqt4-dev \
  libav-tools wget \
  && apt-get autoremove -y && rm -rf /var/lib/apt/lists*

ENV ROOTSYS /usr/local/root
ENV PATH $ROOTSYS/bin:$PATH
ENV PYTHONDIR $PYTHONDIR:$ROOTSYS
ENV LD_LIBRARY_PATH $ROOTSYS/lib:$PYTHONDIR/lib:$ROOTSYS/bindings/pyroot:$LD_LIBRARY_PATH
ENV DYLD_LIBRARY_PATH /usr/lib:$ROOTSYS/lib:$PYTHONDIR/lib:$ROOTSYS/bindings/pyroot:$DYLD_LIBRARY_PATH
ENV PYTHONPATH $PYTHONPATH:$ROOTSYS/lib:$ROOTSYS/bindings/pyroot
RUN mkdir -p /mjsw/ROOT /mjsw/ROOT/build
WORKDIR /mjsw/ROOT
RUN wget --quiet https://github.com/root-project/root/archive/v6-12-04.tar.gz \
  && tar -xf v6-12-04.tar.gz \
  && mv root-6-12-04 root \
  && rm -rf v6-12-04.tar.gz \
  && cd build \
  && cmake ../root -Dgdml=ON -Dminuit2=ON -Droofit=ON -Dhttp=ON -Dldap=OFF -Dfortran=OFF \
    -DCMAKE_CXX_FLAGS=-D_GLIBCXX_USE_CXX11_ABI=0 \
    -DCMAKE_INSTALL_PREFIX:PATH=/usr/local/root \
  && make -j$(nproc); make && make install \
  && rm -rf /mjsw/ROOT/*

ENV CLHEP_VERSION 2.4.0.1
ENV CLHEP_BASE_DIR /mjsw/CLHEP/$CLHEP_VERSION/install
ENV LD_LIBRARY_PATH $CLHEP_BASE_DIR/lib:$LD_LIBRARY_PATH
RUN mkdir -p /mjsw/CLHEP/$CLHEP_VERSION/install /mjsw/CLHEP/$CLHEP_VERSION/build
WORKDIR /mjsw/CLHEP/$CLHEP_VERSION/build
RUN wget -P /mjsw/CLHEP http://proj-clhep.web.cern.ch/proj-clhep/DISTRIBUTION/tarFiles/clhep-$CLHEP_VERSION.tgz \
  && tar zxvf /mjsw/CLHEP/clhep-$CLHEP_VERSION.tgz -C /mjsw/CLHEP/ \
  && cmake -DCMAKE_INSTALL_PREFIX=../install ../CLHEP \
  && cmake --build . --config RelWithDebInfo \
  && cmake --build . --target install \
  && rm -rf * \
  && cd /mjsw/CLHEP/$CLHEP_VERSION \
  && rmdir build \
  && rm /mjsw/CLHEP/clhep-$CLHEP_VERSION.tgz

RUN pip3 install numpy matplotlib pandas notebook

WORKDIR /home/
