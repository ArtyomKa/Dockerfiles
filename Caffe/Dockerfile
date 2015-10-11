
FROM ubuntu



RUN echo "deb http://il.archive.ubuntu.com/ubuntu/ trusty universe\n\
deb-src http://il.archive.ubuntu.com/ubuntu/ trusty universe\n\
deb http://il.archive.ubuntu.com/ubuntu/ trusty-updates universe\n\
deb-src http://il.archive.ubuntu.com/ubuntu/ trusty-updates universe\n"\
>> /etc/apt/sources.list


RUN apt-get update && apt-get -y upgrade 
RUN apt-get install -y \
	build-essential \
	curl \
	git \
	gfortran \
	libatlas-dev \
	libprotobuf-dev \
	libleveldb-dev \
	libsnappy-dev \
	libopencv-dev \
	libhdf5-serial-dev \
	protobuf-compiler \
	python-dev \
	python-numpy \
	liblog4cplus-dev \
	libgflags-dev \
	libgoogle-glog-dev \
	liblmdb-dev \
	--no-install-recommends libboost-all-dev 




WORKDIR /devel
ADD v0.2.14.tar.gz .
RUN cd OpenBLAS-0.2.14 && make -j && make install

ADD caffe-rc2.tar.gz .
RUN pwd && ls -al
WORKDIR /devel/caffe-rc2 
RUN cp Makefile.config.example Makefile.config

RUN sed -i 's|BLAS := atlas|BLAS := open|g' Makefile.config
RUN sed -i 's|# BLAS_INCLUDE := /path/to/your/blas|BLAS_INCLUDE := /opt/OpenBLAS/include|g' Makefile.config
RUN sed -i 's|# BLAS_LIB := .*|BLAS_LIB := /opt/OpenBLAS/lib|g' Makefile.config

RUN pwd
RUN echo "CPU_ONLY := 1" >> Makefile.config
RUN make dist

CMD ["/bin/bash"]
