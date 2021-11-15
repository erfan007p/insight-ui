FROM node:8-alpine

RUN apk add --update --no-cache git \
                                libzmq \
                                zeromq-dev \
                                python \
                                make \
                                g++

WORKDIR /insight

# Copy gobytecore-node
RUN git clone --branch master --single-branch --depth 1 https://github.com/gobytecoin/gobytecore-node.git .

# Copy config file
COPY gobytecore-node.json .

# Install NPM modules
RUN npm ci

ARG VERSION
ARG MAJOR_VERSION

# Install Insight modules
RUN bin/gobytecore-node install @gobytecoin/insight-api@${MAJOR_VERSION}
RUN bin/gobytecore-node install @gobytecoin/insight-ui@${VERSION}

FROM node:8-alpine

LABEL maintainer="GoByte Developers <dev@gobyte.network>"
LABEL description="Dockerised Insight-GoByte"

WORKDIR /insight

# Copy project files
COPY --from=0 /insight/ .

EXPOSE 3001

CMD ["bin/gobytecore-node", "start"]
