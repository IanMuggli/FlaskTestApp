FROM python:3.7.5-slim-buster
LABEL key="Nick Janetakis <nick.janetakis@gmail.com>" 

ENV INSTALL_PATH=/ian
RUN mkdir -p $INSTALL_PATH

WORKDIR $INSTALL_PATH

COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

COPY . .

CMD gunicorn -b 0.0.0.0:8000 --access-logfile - "ian.app:create_app()"

#FROM buildpack-deps:trixie
#
#ENV INSTALL_PATH /ian
#RUN mkdir -p $INSTALL_PATH
#
#COPY requirements.txt requirements.txt
#RUN pip install -r requirements.txt
#
## runtime dependencies
#RUN set -eux; \
#    apt-get update; \
#    apt-get install -y --no-install-recommends \
#    libbluetooth-dev \
#    tk-dev \
#    uuid-dev \
#    ; \
#    apt-get dist-clean
#
#ENV PYTHON_VERSION 3.15.0a7
#ENV PYTHON_SHA256 8f590c428b7f0d406df928b85737e7a3afa28eddd4d1419410ea809687ed1ea7
#
#RUN set -eux; \
#    \
#    savedAptMark="$(apt-mark showmanual)"; \
#    apt-get update; \
#    apt-get install -y --no-install-recommends \
#    libzstd-dev \
#    ; \
#    \
#    wget -O python.tar.xz "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz"; \
#    echo "$PYTHON_SHA256 *python.tar.xz" | sha256sum -c -; \
#    mkdir -p /usr/src/python; \
#    tar --extract --directory /usr/src/python --strip-components=1 --file python.tar.xz; \
#    rm python.tar.xz; \
#    \
#    cd /usr/src/python; \
#    gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)"; \
#    ./configure \
#    --build="$gnuArch" \
#    --enable-loadable-sqlite-extensions \
#    --enable-optimizations \
#    --enable-option-checking=fatal \
#    --enable-shared \
#    $(test "${gnuArch%%-*}" != 'riscv64' && echo '--with-lto') \
#    --with-ensurepip \
#    ; \
#    nproc="$(nproc)"; \
#    EXTRA_CFLAGS="$(dpkg-buildflags --get CFLAGS)"; \
#    LDFLAGS="$(dpkg-buildflags --get LDFLAGS)"; \
#    arch="$(dpkg --print-architecture)"; arch="${arch##*-}"; \
#    # https://docs.python.org/3.12/howto/perf_profiling.html
#    # https://github.com/docker-library/python/pull/1000#issuecomment-2597021615
#    case "$arch" in \
#    amd64|arm64) \
#    # only add "-mno-omit-leaf" on arches that support it
#    # https://gcc.gnu.org/onlinedocs/gcc-14.2.0/gcc/x86-Options.html#index-momit-leaf-frame-pointer-2
#    # https://gcc.gnu.org/onlinedocs/gcc-14.2.0/gcc/AArch64-Options.html#index-momit-leaf-frame-pointer
#    EXTRA_CFLAGS="${EXTRA_CFLAGS:-} -fno-omit-frame-pointer -mno-omit-leaf-frame-pointer"; \
#    ;; \
#    i386) \
#    # don't enable frame-pointers on 32bit x86 due to performance drop.
#    ;; \
#    *) \
#    # other arches don't support "-mno-omit-leaf"
#    EXTRA_CFLAGS="${EXTRA_CFLAGS:-} -fno-omit-frame-pointer"; \
#    ;; \
#    esac; \
#    make -j "$nproc" \
#    "EXTRA_CFLAGS=${EXTRA_CFLAGS:-}" \
#    "LDFLAGS=${LDFLAGS:-}" \
#    ; \
#    # enable GDB to load debugging data: https://github.com/docker-library/python/pull/701
#    bin="$(readlink -ve /usr/local/bin/python3)"; \
#    dir="$(dirname "$bin")"; \
#    mkdir -p "/usr/share/gdb/auto-load/$dir"; \
#    cp -vL Tools/gdb/libpython.py "/usr/share/gdb/auto-load/$bin-gdb.py"; \
#    \
#    cd /; \
#    rm -rf /usr/src/python; \
#    \
#    find /usr/local -depth \
#    \( \
#    \( -type d -a \( -name test -o -name tests -o -name idle_test \) \) \
#    -o \( -type f -a \( -name '*.pyc' -o -name '*.pyo' -o -name 'libpython*.a' \) \) \
#    \) -exec rm -rf '{}' + \
#    ; \
#    \
#    ldconfig; \
#    \
#    apt-mark auto '.*' > /dev/null; \
#    apt-mark manual $savedAptMark; \
#    find /usr/local -type f -executable -not \( -name '*tkinter*' \) -exec ldd '{}' ';' \
#    | awk '/=>/ { so = $(NF-1); if (index(so, "/usr/local/") == 1) { next }; gsub("^/(usr/)?", "", so); printf "*%s\n", so }' \
#    | sort -u \
#    | xargs -rt dpkg-query --search \
#    # https://manpages.debian.org/bookworm/dpkg/dpkg-query.1.en.html#S (we ignore diversions and it'll be really unusual for more than one package to provide any given .so file)
#    | awk 'sub(":$", "", $1) { print $1 }' \
#    | sort -u \
#    | xargs -r apt-mark manual \
#    ; \
#    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
#    apt-get dist-clean; \
#    \
#    export PYTHONDONTWRITEBYTECODE=1; \
#    python3 --version; \
#    pip3 --version
#
#CMD ["python3"]
#