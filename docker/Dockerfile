    FROM node:15.9.0-alpine3.12
    ARG JD_SHELL=https://github.com/borrrrring/tsnode.git
    ARG JD_SHELL_BRANCH=master
    ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin \
        LANG=zh_CN.UTF-8 \
        SHELL=/bin/bash \
        PS1="\u@\h:\w \$ " \
        JD_DIR=/jd \
        ENABLE_TTYD=true \
        ENABLE_WEB_PANEL=true \
        ENABLE_HANGUP=false \
        ENABLE_RESET_REPO_URL=true \
        JD_SHELL_URL=https://github.com/borrrrring/tsnode.git \
        JD_SCRIPTS_URL=git@gitee.com:lxk0301/jd_scripts.git
    WORKDIR ${JD_DIR}
    RUN apk update -f \
        && apk upgrade \
        && apk --no-cache add -f bash \
                                 coreutils \
                                 git \
                                 wget \
                                 curl \
                                 nano \
                                 tzdata \
                                 perl \
                                 openssh-client \
                                 python3 \
                                 py3-pip \
                                 zlib-dev \
                                 jpeg-dev \
        && apk add --no-cache --virtual .build \
                     gcc \
                     python3-dev \
                     musl-dev \
        && rm -rf /var/cache/apk/* \
        && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
        && echo "Asia/Shanghai" > /etc/timezone \
        && git clone -b $JD_SHELL_BRANCH $JD_SHELL $JD_DIR \
        && npm install \
        && npm install -g pm2 \
        && ln -sf $JD_DIR/jtask.sh /usr/local/bin/jd \
        && ln -sf $JD_DIR/jtask.sh /usr/local/bin/jtask \
        && ln -sf $JD_DIR/jtask.sh /usr/local/bin/otask \
        && ln -sf $JD_DIR/jtask.sh /usr/local/bin/mtask \
        && ln -sf $JD_DIR/jup.sh /usr/local/bin/git_pull \
        && ln -sf $JD_DIR/jup.sh /usr/local/bin/jup \
        && ln -sf $JD_DIR/jlog.sh /usr/local/bin/rm_log \
        && ln -sf $JD_DIR/jlog.sh /usr/local/bin/jlog \
        && ln -sf $JD_DIR/jcode.sh /usr/local/bin/export_sharecodes \
        && ln -sf $JD_DIR/jcode.sh /usr/local/bin/jcode \
        && ln -sf $JD_DIR/jcsv.sh /usr/local/bin/jcsv \
        && ln -sf /usr/bin/python3 /usr/bin/python \
        && pip3 install --upgrade pip \
        && cd $JD_DIR/jbot \
        && pip3 install -r requirements.txt \
        && cp -f $JD_DIR/docker/docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh \
        && chmod 777 /usr/local/bin/docker-entrypoint.sh \
        && chmod 777 $JD_DIR/*.sh \
        && rm -rf /root/.npm \
        && apk del .build \
        && rm -rf /root/.cache \
        &&  npm i -g ttyd \
        && npm i -S png-js \
        && npm i -g cnpm \
        && cnpm i \
        && cnpm i -g typescript  \
        && cnpm i -g ts-node \
        && pip3 install requests \
        && mkdir -p /root/.ssh \
        && echo -e 'need to complete……' > /root/.ssh/id_rsa \
        && chmod 600 /root/.ssh/id_rsa \
        && ssh-keyscan gitee.com > /root/.ssh/known_hosts
    WORKDIR ${JD_DIR}/app
    RUN yarn
    EXPOSE 6789
    CMD [ "node", "index.js" ]
    WORKDIR ${JD_DIR}
    ENTRYPOINT ["docker-entrypoint.sh"]
