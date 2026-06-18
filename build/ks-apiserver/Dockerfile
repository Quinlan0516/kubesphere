# Copyright 2020 The KubeSphere Authors. All rights reserved.
# Use of this source code is governed by an Apache license
# that can be found in the LICENSE file.

# Build
FROM golang:1.19.4 as build_context

ENV OUTDIR=/out
RUN mkdir -p ${OUTDIR}/usr/local/bin/

WORKDIR /workspace
ADD . /workspace/

# Fix: recreate symlinks for staging modules (lost on Windows clone)
RUN rm -rf /workspace/vendor/kubesphere.io/api \
    /workspace/vendor/kubesphere.io/client-go \
    /workspace/vendor/kubesphere.io/utils && \
    ln -s ../../staging/src/kubesphere.io/api /workspace/vendor/kubesphere.io/api && \
    ln -s ../../staging/src/kubesphere.io/client-go /workspace/vendor/kubesphere.io/client-go && \
    ln -s ../../staging/src/kubesphere.io/utils /workspace/vendor/kubesphere.io/utils

RUN make ks-apiserver
RUN mv /workspace/bin/cmd/ks-apiserver ${OUTDIR}/usr/local/bin/

##############
# Final image
#############

FROM alpine:3.16.2

COPY --from=build_context /out/ /

WORKDIR /

EXPOSE 9090
CMD ["sh"]
