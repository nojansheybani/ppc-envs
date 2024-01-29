FROM python:3.10.3-bullseye as buildenv

RUN apt-get update && apt-get install -y --no-install-recommends \
                automake \
                build-essential \
                clang-11 \
		        cmake \
                git \
                libboost-dev \
                libboost-thread-dev \
                libclang-dev \
                libgmp-dev \
                libntl-dev \
                libsodium-dev \
                libssl-dev \
                libtool \
                vim \
                gdb \
                valgrind \
        && rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/data61/MP-SPDZ.git
ENV MP_SPDZ_HOME ./MP-SPDZ
WORKDIR $MP_SPDZ_HOME

RUN pip install --upgrade pip ipython

ARG arch=native
ARG cxx=clang++-11
ARG use_ntl=0
ARG prep_dir="Player-Data"
ARG ssl_dir="Player-Data"

RUN echo "ARCH = -march=${arch}" >> CONFIG.mine \
        && echo "CXX = ${cxx}" >> CONFIG.mine \
        && echo "USE_NTL = ${use_ntl}" >> CONFIG.mine \
        && echo "MY_CFLAGS += -I/usr/local/include" >> CONFIG.mine \
        && echo "MY_LDLIBS += -Wl,-rpath -Wl,/usr/local/lib -L/usr/local/lib" \
            >> CONFIG.mine \
        && mkdir -p $prep_dir $ssl_dir \
        && echo "PREP_DIR = '-DPREP_DIR=\"${prep_dir}/\"'" >> CONFIG.mine \
        && echo "SSL_DIR = '-DSSL_DIR=\"${ssl_dir}/\"'" >> CONFIG.mine

RUN make boost libote

RUN pip install numpy

# ssl keys
ARG cryptoplayers=3
ENV PLAYERS ${cryptoplayers}
RUN ./Scripts/setup-ssl.sh ${cryptoplayers} ${ssl_dir}

ADD "https://www.random.org/cgi-bin/randbyte?nbytes=10&format=h" skipcache
COPY average.mpc Programs/Source/average.mpc

RUN Scripts/compile-run.py -E rep-field average -F 16 -X -Y
# RUN Scripts/compile-run.py -E atlas array_gen