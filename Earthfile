VERSION 0.6
IMPORT ./prereqs AS prereqs
IMPORT ./python AS python

# Required for pushing new images
ARG BRANCH
ARG CODECS_INSTALL_PREFIX="/opt/codecs"

python-base:
    ARG --required OS_TARGET
    ARG --required PY_VERSION
    ARG ALPINE_VERSION
    ARG DEBIAN_VERSION
    ARG --required NUMPY_VERSION
    ARG --required LIBJPEGTURBO_VERSION
    ARG --required LIBPNG_VERSION
    ARG --required LIBWEBP_VERSION
    ARG --required OPENCV_VERSION
    FROM python+python-base \
        --BRANCH=${BRANCH} \
        --NUMPY_VERSION=${NUMPY_VERSION} \
        --OS_TARGET=${OS_TARGET} \
        --PY_VERSION=${PY_VERSION} \
        --ALPINE_VERSION=${ALPINE_VERSION} \
        --DEBIAN_VERSION=${DEBIAN_VERSION} \
        --LIBJPEGTURBO_VERSION=${LIBJPEGTURBO_VERSION} \
        --LIBPNG_VERSION=${LIBPNG_VERSION} \
        --LIBWEBP_VERSION=${LIBWEBP_VERSION} \
        --OPENCV_VERSION=${OPENCV_VERSION} \
        --CODECS_INSTALL_PREFIX=${CODECS_INSTALL_PREFIX}
    SAVE IMAGE foo:latest
    SAVE ARTIFACT ${CODECS_INSTALL_PREFIX}
    SAVE ARTIFACT /build/opencv

prereqs-base:
    ARG --required OS_TARGET
    ARG ALPINE_VERSION
    ARG DEBIAN_VERSION
    ARG --required LIBJPEGTURBO_VERSION
    ARG --required LIBPNG_VERSION
    ARG --required LIBWEBP_VERSION
    ARG --required OPENCV_VERSION
    FROM prereqs+base-image \
        --BRANCH=${BRANCH} \
        --OS_TARGET=${OS_TARGET} \
        --ALPINE_VERSION=${ALPINE_VERSION} \
        --DEBIAN_VERSION=${DEBIAN_VERSION} \
        --LIBJPEGTURBO_VERSION=${LIBJPEGTURBO_VERSION} \
        --LIBPNG_VERSION=${LIBPNG_VERSION} \
        --LIBWEBP_VERSION=${LIBWEBP_VERSION} \
        --OPENCV_VERSION=${OPENCV_VERSION} \
        --CODECS_INSTALL_PREFIX=${CODECS_INSTALL_PREFIX}
    SAVE ARTIFACT ${CODECS_INSTALL_PREFIX}
    SAVE ARTIFACT /build/opencv


foo-base:
    ARG --required OS_TARGET
    ARG --required PY_VERSION
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


