VERSION 0.6
IMPORT ./prereqs AS prereqs
ARG LIBWEBP_VERSION="v1.2.0"
ARG CODECS_INSTALL_PREFIX="/opt/codecs"




base-image:
    ARG --required OS_TARGET
    ARG --required PYTHON_VERSION_MINOR
    FROM busybox
    IF [ "${OS_TARGET}" = "alpine" ] 
        ARG --required ALPINE_VERSION
        FROM python:${PYTHON_VERSION_MINOR}-alpine${ALPINE_VERSION}
    ELSE 
        IF [ "${OS_TARGET}" = "debian" ]
            ARG --required DEBIAN_VERSION
            FROM python:${PYTHON_VERSION_MINOR}-slim-${DEBIAN_VERSION}
        ELSE
            RUN echo "Invalid OS provided." 1>&2 && exit 1
        END
    END
    COPY +libjpeg-turbo/codecs ${CODECS_INSTALL_PREFIX}
    COPY +libpng/codecs ${CODECS_INSTALL_PREFIX}
    COPY +libwebp/codecs ${CODECS_INSTALL_PREFIX}
    COPY +opencv/opencv /build/opencv
    SAVE IMAGE pathds/base:latest


opencv:
    ARG --required OPENCV_VERSION
    ARG --required OS_TARGET
    FROM +prereqs-${OS_TARGET}
    DO prereqs+BUILD_OPENCV --OPENCV_VERSION=${OPENCV_VERSION}


libjpeg-turbo:
    ARG --required LIBJPEGTURBO_VERSION
    ARG --required OS_TARGET
    FROM +prereqs-${OS_TARGET}
    DO prereqs+BUILD_LIBJPEG_TURBO --LIBJPEGTURBO_VERSION=${LIBJPEGTURBO_VERSION} --CODECS_INSTALL_PREFIX=${CODECS_INSTALL_PREFIX}

libpng:
    ARG --required LIBPNG_VERSION
    ARG --required OS_TARGET
    FROM +prereqs-${OS_TARGET}
    DO prereqs+BUILD_LIBPNG --LIBPNG_VERSION=${LIBPNG_VERSION} --CODECS_INSTALL_PREFIX=${CODECS_INSTALL_PREFIX}

libwebp:
    ARG --required LIBWEBP_VERSION
    ARG --required OS_TARGET
    FROM +prereqs-${OS_TARGET}
    DO prereqs+BUILD_LIBWEBP --LIBWEBP_VERSION=${LIBWEBP_VERSION} --CODECS_INSTALL_PREFIX=${CODECS_INSTALL_PREFIX}

prereqs-alpine:
    ARG --required ALPINE_VERSION
    FROM alpine:${ALPINE_VERSION}
    DO prereqs+BUILD_ALPINE_PREREQ

prereqs-debian:
    ARG --required DEBIAN_VERSION
    FROM debian:${DEBIAN_VERSION}-slim
    DO prereqs+BUILD_DEBIAN_PREREQ
