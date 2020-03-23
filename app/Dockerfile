FROM php:7

COPY ./moodle /moodle

RUN chown -R root /moodle
RUN chmod -R 0755 /moodle
RUN find /moodle -type f -exec chmod 0644 {} \;

