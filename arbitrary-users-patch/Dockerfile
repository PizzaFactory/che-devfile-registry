ARG FROM_IMAGE
FROM ${FROM_IMAGE}
USER 0
# Set permissions on /etc/passwd and /home to allow arbitrary users to write
COPY --chown=0:0 entrypoint.sh /
RUN mkdir -p /home/user && chgrp -R 0 /home && chmod -R g=u /etc/passwd /etc/group /home && chmod +x /entrypoint.sh && \
    if [ -e /etc/shadow ]; then chgrp -R 0 /etc/shadow && chmod g=u /etc/shadow; fi

# Install common tools in container to aid development process
COPY install-tooling.sh /tmp
RUN /tmp/install-tooling.sh && rm -f /tmp/install-tooling.sh

USER 10001
ENV HOME=/home/user
WORKDIR /projects
ENTRYPOINT [ "/entrypoint.sh" ]
CMD ["tail", "-f", "/dev/null"]

LABEL "che.base.image" ${FROM_IMAGE}
LABEL "builder" "PizzaFactory/Camino"
