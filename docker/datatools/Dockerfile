FROM debian as build

RUN apt update -y && apt install -y linux-perf gdb strace tcpdump procps coreutils curl openssl && mkdir -p /tools
WORKDIR /tools
COPY mongodb-mongosh_2.1.5_amd64.deb /tools/mongodb-mongosh_2.1.5_amd64.deb
RUN dpkg -i mongodb-mongosh_2.1.5_amd64.deb
COPY mongodb-atlas-cli_1.16.0_linux_x86_64.deb /tools/mongodb-atlas-cli_1.16.0_linux_x86_64.deb
RUN dpkg -i mongodb-atlas-cli_1.16.0_linux_x86_64.deb 
COPY mongocli_1.31.3_linux_x86_64.deb /tools/mongocli_1.31.3_linux_x86_64.deb
RUN dpkg -i mongocli_1.31.3_linux_x86_64.deb
COPY mongodb-database-tools-debian11-x86_64-100.9.4.deb /tools/mongodb-database-tools-debian11-x86_64-100.9.4.deb
RUN dpkg -i mongodb-database-tools-debian11-x86_64-100.9.4.deb 

FROM debian
RUN apt update -y && apt install -y linux-perf gdb strace tcpdump procps coreutils curl openssl && mkdir -p /tools
WORKDIR /tools
COPY --from=build /usr/bin/mongo* /usr/bin/
COPY --from=build /etc/atlascli /etc/atlascli
COPY --from=build /usr/bin/atlas /usr/bin/atlas
COPY --from=build /usr/bin/bsondump /usr/bin/bsondump
#COPY --from=build /usr/lib/mongosh_crypt_v1.so /usr/lib/mongosh_crypt_v1.so
COPY --from=build /usr/share/doc/mongodb-database-tools /usr/share/doc/mongodb-database-tools
COPY --from=build /usr/share/doc/mongodb-mongosh /usr/share/doc/mongodb-mongosh
COPY --from=build /usr/share/man/man1/mongosh.1.gz /usr/share/man/man1/mongosh.1.gz
CMD bash
