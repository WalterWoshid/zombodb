#
# Copyright 2018-2023 ZomboDB, LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
FROM fedora:43

LABEL maintainer="ZomboDB, LLC (zombodb@gmail.com)"

ARG USER=docker
ARG UID=1000
ARG GID=1000


RUN useradd -m ${USER} --uid=${UID}

RUN dnf update -y
RUN dnf install -y \
    git \
    https://download.postgresql.org/pub/repos/yum/reporpms/F-43-x86_64/pgdg-fedora-repo-latest.noarch.rpm \
    postgresql13-server postgresql13-devel \
    postgresql14-server postgresql14-devel \
    postgresql15-server postgresql15-devel \
    postgresql16-server postgresql16-devel \
    postgresql17-server postgresql17-devel \
    openssl-devel \
    redhat-rpm-config \
    ruby-devel \
    gcc \
    make \
    rpm-build \
    libffi-devel

RUN dnf group install -y "Development Tools"
# TODO RUN dnf update -y

RUN gem install --no-document fpm


USER ${UID}:${GID}
WORKDIR /home/${USER}
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | bash -s -- -y
ENV PATH="/home/${USER}/.cargo/bin:${PATH}"

RUN cargo install cargo-pgrx --version 0.13.1 --locked
RUN cargo pgrx init \
	--pg13=/usr/pgsql-13/bin/pg_config \
	--pg14=/usr/pgsql-14/bin/pg_config \
	--pg15=/usr/pgsql-15/bin/pg_config \
    --pg16=/usr/pgsql-16/bin/pg_config \
    --pg17=/usr/pgsql-17/bin/pg_config
