FROM openshift/base-centos7

# TODO: Put the maintainer name in the image metadata
MAINTAINER Abhishek koserwal <akoserwa@redhat.com>


ENV NPM_RUN=start \
    NODE_VERSION=8.0.0 \
    NPM_CONFIG_LOGLEVEL=info \
    NPM_CONFIG_PREFIX=$HOME/.npm-global \
    PATH=$HOME/node_modules/.bin/:$HOME/.npm-global/bin/:$PATH \
    DEBUG_PORT=5858 \
    NODE_ENV=production \
    DEV_MODE=false


ENV NGINX_VERSION 1.8
ENV NGINX_BASE_DIR /opt/nginx/root
ENV NGINX_VAR_DIR /var/opt/nginx

LABEL io.k8s.description="Platform for building Node.js and running application over Nginx" \
      io.k8s.display-name="Node.js v$NODE_VERSION & nginx builder ${NGINX_VERSION}" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,nodejs,nodejs$NODE_VERSION" \
      com.redhat.deployments-dir="/opt/app-root/src"

USER root

# TODO: Install required packages here:
# RUN yum install -y ... && yum clean all -y
RUN yum install --setopt=tsflags=nodocs -y centos-release-scl-rh \
 && yum install --setopt=tsflags=nodocs -y bcrypt nginx${NGINX_VERSION/\./} \
 && yum clean all -y \
 && mkdir -p /opt/app-root/etc/nginx.conf.d /opt/app-root/run \
 && chmod -R a+rx  $NGINX_VAR_DIR/lib/nginx \
 && chmod -R a+rwX $NGINX_VAR_DIR/lib/nginx/tmp \
                   $NGINX_VAR_DIR/log \
                   $NGINX_VAR_DIR/run \
                   /opt/app-root/run


RUN curl -sL https://rpm.nodesource.com/setup_8.x | bash -
RUN yum -y install nodejs && yum clean all -y

  

# TODO (optional): Copy the builder files into /opt/app-root
# COPY ./<builder_folder>/ /opt/app-root/
COPY ./etc/ /opt/app-root/etc

COPY ./s2i/bin/ /usr/local/s2i


# TODO: Copy the S2I scripts to /usr/libexec/s2i, since openshift/base-centos7 image
# sets io.openshift.s2i.scripts-url label that way, or update that label
COPY ./s2i/bin/ /usr/libexec/s2i

# TODO: Drop the root user and make the content of /opt/app-root owned by user 1001
RUN cp /opt/app-root/etc/nginx.server.sample.conf /opt/app-root/etc/nginx.conf.d/default.conf \
 && chown -R 1001:1001 /opt/app-root


# This default user is created in the openshift/base-centos7 image
USER 1001


# TODO: Set the default port for applications built using this image
EXPOSE 8080

# TODO: Set the default CMD for the image
CMD ["usage"]
