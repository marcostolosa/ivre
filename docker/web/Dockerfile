# This file is part of IVRE.
# Copyright 2011 - 2023 Pierre LALET <pierre@droids-corp.org>
#
# IVRE is free software: you can redistribute it and/or modify it
# under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# IVRE is distributed in the hope that it will be useful, but WITHOUT
# ANY WARRANTY; without even the implied warranty of MERCHANTABILITY
# or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public
# License for more details.
#
# You should have received a copy of the GNU General Public License
# along with IVRE. If not, see <http://www.gnu.org/licenses/>.

FROM debian:stable
LABEL maintainer="Pierre LALET <pierre@droids-corp.org>"

RUN apt-get -q update && \
    apt-get -qy --no-install-recommends install nginx && \
    apt-get clean && rm -rf /var/lib/apt/lists/*
COPY nginx-default-site /etc/nginx/sites-available/default

RUN apt-get -q update && \
    apt-get -qy --no-install-recommends install git ca-certificates && \
    git clone https://github.com/ivre/ivre && \
    mkdir -p /usr/local/share/ivre/web && \
    mv ivre/web/static /usr/local/share/ivre/web/ && \
    rm -rf ivre/ && \
    apt-get -qy --purge autoremove git ca-certificates && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /var/www/dokuwiki/ && \
    ln -sf /var/www/dokuwiki/data/lib /var/www/dokuwiki/

EXPOSE 80
CMD ["nginx", "-g", "daemon off; error_log stderr;"]
