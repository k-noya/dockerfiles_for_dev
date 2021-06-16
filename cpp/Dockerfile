#
# Docker image for development with C++
#
FROM gcc:10.2

# Install CMake.
RUN apt update && apt install -y cmake

# Install python-dev (required by Boost).
RUN apt update && apt install -y python-dev

# Build and install Boost from source code.
# The following libraries aren't built.
#  - graph_parallel
#  - mpi
ENV BOOST_WORK_DIR=/tmp/boost_build
WORKDIR ${BOOST_WORK_DIR}

ENV BOOST_VERSION=1.75.0
ENV BOOST_INSTALL_DIR=/usr/local

RUN export BOOST_DIR_NAME=boost_$(echo ${BOOST_VERSION} | sed -e 's/\./_/g') \
  && export BOOST_ARCHIVE_FILE=${BOOST_DIR_NAME}.tar.gz \
  && export BOOST_SOURCE_URL=https://boostorg.jfrog.io/artifactory/main/release/${BOOST_VERSION}/source/${BOOST_ARCHIVE_FILE} \
  && wget -q ${BOOST_SOURCE_URL} \
  && tar zxf ${BOOST_ARCHIVE_FILE} \
  && cd ${BOOST_DIR_NAME} \
  && ./bootstrap.sh --prefix=${BOOST_INSTALL_DIR} \
  && ./b2 install \
       --without-graph_parallel \
       --without-mpi \
  && cd ${BOOST_WORK_DIR}/../ \
  && rm -rf ${BOOST_WORK_DIR}

WORKDIR /

# Install llvm-toolchain and clang-format
ENV LLVM_VERSION=10
RUN apt update \
  && apt install -y software-properties-common \
  && wget https://apt.llvm.org/llvm.sh \
  && chmod +x llvm.sh \
  && ./llvm.sh ${LLVM_VERSION} \
  && apt update \
  && apt install -y clang-format-${LLVM_VERSION} \
  && ln -s /usr/bin/clang-format-${LLVM_VERSION} /usr/bin/clang-format

# Install cpplint
RUN apt update \
  && apt install -y python3-pip \
  && python3 -m pip install cpplint

# Install googletest
RUN apt update \
  && apt install -y libgtest-dev
