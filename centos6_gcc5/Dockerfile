FROM centos:centos6

# Install repo GCC
RUN yum install -y perl gcc gcc-c++ wget xz libgcc glibc-devel glibc-headers git && \
    yum -y clean all

# Test repo GCC
RUN gcc --version && \
    g++ --version && \
    which gcc && \
    which g++
RUN mkdir /tmp/tests && \
    cd /tmp/tests && \
    echo '#include <iostream>' > main.cpp && \
    echo 'using namespace std;' >> main.cpp && \
    echo 'int main() {' >> main.cpp && \
    echo '    cout << "Hello world!" << endl;' >> main.cpp && \
    echo '    return 0;' >> main.cpp && \
    echo '}' >> main.cpp && \
    g++ main.cpp -o main && \
    ./main

### Download and compile GCC-5 ###
# http://en.librehat.com/blog/build-gcc-5-dot-2-on-rhel-6/

# Download and extract source code
ENV GCC_VERSION "5.2.0"
RUN cd /tmp && \
    wget --no-verbose \
        http://ftpmirror.gnu.org/gcc/gcc-${GCC_VERSION}/gcc-${GCC_VERSION}.tar.bz2 && \
    tar xf gcc-${GCC_VERSION}.tar.bz2
RUN cd /tmp && \
    wget --no-verbose \
        https://gmplib.org/download/gmp/gmp-6.1.0.tar.xz && \
    tar xf gmp-6.1.0.tar.xz && \
    mv gmp-6.1.0 gcc-${GCC_VERSION}/gmp
RUN cd /tmp && \
    wget --no-verbose \
        ftp://ftp.gnu.org/gnu/mpc/mpc-1.0.3.tar.gz && \
    tar xf mpc-1.0.3.tar.gz && \
    mv mpc-1.0.3 gcc-${GCC_VERSION}/mpc
RUN cd /tmp && \
    wget --no-verbose \
        http://www.mpfr.org/mpfr-current/mpfr-3.1.4.tar.xz && \
    tar xf mpfr-3.1.4.tar.xz && \
    mv mpfr-3.1.4 gcc-${GCC_VERSION}/mpfr

# Compile and install GCC
# "we highly recommend that GCC be built into a separate directory from the sources which does not reside within the source tree"
RUN mkdir /tmp/gcc-${GCC_VERSION}_build && \
    cd /tmp/gcc-${GCC_VERSION}_build && \
    ../gcc-${GCC_VERSION}/configure \
        --prefix=/usr \
        --disable-multilib \
        --enable-languages=c,c++ \
        --enable-libstdcxx-threads \
        --enable-libstdcxx-time \
        --enable-shared \
        --enable-__cxa_atexit \
        --disable-libunwind-exceptions \
        --disable-libada \
        --host x86_64-linux-gnu \
        --build x86_64-linux-gnu \
        --with-default-libstdcxx-abi=gcc4-compatible
RUN cd /tmp/gcc-${GCC_VERSION}_build && make -j$(nproc)
RUN cd /tmp/gcc-${GCC_VERSION}_build && make install

# Test GCC again
RUN gcc --version && \
    g++ --version && \
    which gcc && \
    which g++
RUN cd /tmp/tests && \
    g++ main.cpp -o main && \
    ./main

# Register new libraries with `ldconfig`
RUN echo "/usr/local/lib64" > usrLocalLib64.conf && \
    mv usrLocalLib64.conf /etc/ld.so.conf.d/ && \
    ldconfig

# Clean out all the garbage
RUN rm -rf /tmp/*

# Default command
CMD ["/bin/bash"]

