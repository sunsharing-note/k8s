FROM rabbitmq:3.6.11-management-alpine

MAINTAINER test

RUN apk update && apk add ca-certificates && \
    apk add tzdata && \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    echo "Asia/Shanghai" > /etc/timezone
    

ADD plugins/*.ez /opt/rabbitmq/plugins/
RUN rabbitmq-plugins enable --offline autocluster
