FROM mjsw-base
#
# set environment variables
#
ENV CLHEP_INCLUDE_DIR ${CLHEP_BASE_DIR}/include
ENV CLHEP_LIB_DIR ${CLHEP_BASE_DIR}/lib
#
ENV MJSWVERSION "mjsw202206Dev"
ENV MJSWDIR /mjsw
ENV MJSWDEV /mjsw
ENV ORDIR /mjsw/OrcaRoot
ENV MGDODIR /mjsw/MGDO
ENV TAMDIR ${MGDODIR}/tam
ENV GATDIR /mjsw/GAT
ENV LD_LIBRARY_PATH ${ORDIR}/lib:${MGDODIR}/lib:${GATDIR}/lib:${TAMDIR}/lib:${LD_LIBRARY_PATH}
ENV PATH /mjsw/mjrdb_Utils:${ORDIR}/Applications:/mjsw/MJOR/:${MGDODIR}/bin:${GATDIR}/Apps:${GATDIR}/Scripts:${PATH}
ENV ROOT_INCLUDE_PATH ${CLHEP_INCLUDE_DIR}:$MGDODIR/Base:$MGDODIR/Gerda:$MGDODIR/GerdaTransforms:$MGDODIR/Majorana:$MGDODIR/MJDB:$MGDODIR/Root:$MGDODIR/Tabree:$MGDODIR/Tools:$MGDODIR/Transforms:$TAMDIR:$TAMDIR/inc:$GATDIR/BaseClasses:$GATDIR/MGTEventProcessing:$GATDIR/MGOutputMCRunProcessing:$GATDIR/SiggenWrapper
#
# bake in these paths for MJDDATADIR, PYTHONDIR - good only on NERSC systems
# user will need to re-define for local machines when necessary
#
ENV MJDDATADIR /global/project/projectdirs/majorana/data/mjd
ENV PYTHONPATH $PYTHONPATH:${GATDIR}/Scripts:/global/homes/m/mjd/production/Utils
ENV PATH /global/homes/m/mjd/production/Utils:${PATH}
#
# update mjd software from git repo to local directory before build
# see /Users/tedeschi/Documents/Majorana/docker/mjdsw/update_mjdsw.sh
# copy versions of the mjd software from local directory into container
# this way the git password stays out of this file and the container
#
# MGDO
#
RUN mkdir -p /mjsw/MGDO
COPY MGDO /mjsw/MGDO/.
WORKDIR $MGDODIR
RUN ./configure  --prefix=`pwd` --enable-majorana-all CXXFLAGS=-std=c++11 \
  && make
#
# add siggen
#
RUN mkdir -p /mjsw/icpc_siggen
COPY icpc_siggen /mjsw/icpc_siggen/.
#ENV SIGGENDIR /mjsw/icpc_siggen
#
# GAT
#
RUN mkdir -p /mjsw/GAT
COPY GAT /mjsw/GAT/.
WORKDIR $GATDIR
RUN make clean \
  && make

#
#WORKDIR /mjsw
