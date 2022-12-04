# Log files

## Syslog

Contains messages that are recorded by the host about system activity:

    cat /var/log/syslog* | head

## Auth Logs

Information about users and authentication-related logs:

    cat /var/log/auth.log* |head

## Third-party logs

`/var/log` contains logs for third-party applications, for example Apache2 logs:

    ls /var/log/apache2/
