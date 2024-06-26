FROM alpine:3.19

LABEL author="Alexandre Cassen <acassen@gmail.com>"
LABEL project="https://github.com/acassen/keepalived"
LABEL homepage="https://www.keepalived.org"

ARG GIT_KVER=master
ARG MAP_FROM='local'

ARG __ENABLE_MAGIC__=0
ARG __ENABLE_DBUS__=0
ARG __DISABLE_IPSET__=0
ARG __DISABLE_IPTABLES__=0
ARG __DISABLE_NFTABLES__=0
ARG __ENABLE_SNMP_VRRP__=0
ARG __ENABLE_REGEX__=0
ARG __ENABLE_REGEX_TIMERS__=0
ARG __ENABLE_JSON__=0
ARG __DISABLE_LVS__=0
ARG __DISABLE_VRRP__=0
ARG __DISABLE_VRRP_AUTH__=0
ARG __ENABLE_BFD__=0
ARG __DISABLE_VMAC__=0
ARG __ENABLE_LTO__=0
ARG __DISABLE_CHECKSUM_COMPACT__=0
ARG __DISABLE_ROUTES__=0
ARG __DISABLE_LINKBEAT__=0
ARG __ENABLE_SOCKADDR_STORAGE__=0
ARG __DISABLE_FWMARK__=0
ARG __DISABLE_TRACK_PROCESS__=0
ARG __ENABLE_MEM_CHECK__=0
ARG __ENABLE_DEBUG__=0
ARG __ENABLE_SNMP_ALERT_DEBUG__=0
ARG __ENABLE_EPOLL_DEBUG__=0
ARG __ENABLE_VRRP_FD_DEBUG__=0
ARG __ENABLE_RECVMSG_DEBUG__=0
ARG __ENABLE_EINTR_DEBUG__=0
ARG __ENABLE_PARSER_DEBUG__=0
ARG __ENABLE_CHECKSUM_DEBUG__=0
ARG __ENABLE_CHECKER_DEBUG__=0
ARG __ENABLE_MEM_ERR_DEBUG__=0
ARG __ENABLE_SCRIPT_DEBUG__=0

ARG WDIR=/usr/src/keepalived
ARG CPY=/usr/src/configure.py
ARG CJSON=/usr/src/configure.json

ADD ./configure/configure.py $CPY
ADD ./configure/configure.json $CJSON
ADD ./keepalived-wrapper /usr/local/bin/keepalived-wrapper

WORKDIR /

RUN \
\
set -e; \
\
apk --no-cache add \
        python3 \
        git \
        binutils \
        gcc \
        libnl3 \
        libnl3-dev \
        make \
        musl-dev \
        openssl \
        openssl-dev \
        autoconf \
        automake \
        libnfnetlink \
        libnfnetlink-dev \
        curl \
        libip4tc \
        libip6tc; \
\
if [[ $__ENABLE_MAGIC__     -eq 1 ]]; then apk --no-cache add file file-dev;           fi; \
if [[ $__ENABLE_DBUS__      -eq 1 ]]; then apk --no-cache add glib glib-dev;           fi; \
if [[ $__DISABLE_IPSET__    -eq 0 ]]; then apk --no-cache add ipset ipset-dev;         fi; \
if [[ $__DISABLE_IPTABLES__ -eq 0 ]]; then apk --no-cache add iptables iptables-dev;   fi; \
if [[ $__DISABLE_NFTABLES__ -eq 0 ]]; then apk --no-cache add libmnl-dev libnftnl-dev; fi; \
if [[ $__ENABLE_SNMP_VRRP__ -eq 1 ]]; then apk --no-cache add net-snmp-dev;            fi; \
if [[ $__ENABLE_REGEX__     -eq 1 ]]; then apk --no-cache add pcre2 pcre2-dev;         fi; \
\
git clone -b $GIT_KVER https://github.com/acassen/keepalived.git $WDIR; \
\
cd $WDIR; \
\
mv $CPY $CJSON $WDIR; \
\
if [[ ! $MAP_FROM == "local" ]]; then wget $MAP_FROM -O ./configure/configure.json; fi; \
\
./autogen.sh; \
\
chmod +x ./configure.py /usr/local/bin/keepalived-wrapper; \
\
./configure.py \
        --enable-dbus              $__ENABLE_DBUS__ \
        --disable-libipset         $__DISABLE_IPSET__ \
        --disable-iptables         $__DISABLE_IPTABLES__ \
        --disable-nftables         $__DISABLE_NFTABLES__ \
        --enable-snmp-vrrp         $__ENABLE_SNMP_VRRP__ \
        --enable-regex             $__ENABLE_REGEX__ \
        --enable-regex-timers      $__ENABLE_REGEX_TIMERS__ \
        --enable-json              $__ENABLE_JSON__ \
        --disable-lvs              $__DISABLE_LVS__ \
        --disable-vrrp             $__DISABLE_VRRP__ \
        --disable-vrrp-auth        $__DISABLE_VRRP_AUTH__ \
        --enable-bfd               $__ENABLE_BFD__ \
        --disable-vmac             $__DISABLE_VMAC__ \
        --enable-lto               $__ENABLE_LTO__ \
        --disable-checksum-compact $__DISABLE_CHECKSUM_COMPACT__ \
        --disable-routes           $__DISABLE_ROUTES__ \
        --disable-linkbeat         $__DISABLE_LINKBEAT__ \
        --enable-sockaddr-storage  $__ENABLE_SOCKADDR_STORAGE__ \
        --disable-fwmark           $__DISABLE_FWMARK__ \
        --enable-mem-check         $__ENABLE_MEM_CHECK__ \
        --enable-debug             $__ENABLE_DEBUG__ \
        --disable-track-process    $__DISABLE_TRACK_PROCESS__ \
        --enable-snmp-alert-debug  $__ENABLE_SNMP_ALERT_DEBUG__ \
        --enable-epoll-debug       $__ENABLE_EPOLL_DEBUG__ \
        --enable-vrrp-fd-debug     $__ENABLE_VRRP_FD_DEBUG__ \
        --enable-recvmsg-debug     $__ENABLE_RECVMSG_DEBUG__ \
        --enable-eintr-debug       $__ENABLE_EINTR_DEBUG__ \
        --enable-parser-debug      $__ENABLE_PARSER_DEBUG__ \
        --enable-checksum-debug    $__ENABLE_CHECKSUM_DEBUG__ \
        --enable-checker-debug     $__ENABLE_CHECKER_DEBUG__ \
        --enable-mem-err-debug     $__ENABLE_MEM_ERR_DEBUG__ \
        --enable-script-debug      $__ENABLE_SCRIPT_DEBUG__; \
\
make && make install; \
\
apk --no-cache del \
        python3 \
        git \
        binutils \
        gcc \
        libnl3-dev \
        make \
        musl-dev \
        openssl-dev \
        autoconf \
        automake \
        libnfnetlink-dev; \
\
if [[ $__ENABLE_MAGIC__     -eq 1 ]]; then apk --no-cache del file-dev;                fi; \
if [[ $__ENABLE_DBUS__      -eq 1 ]]; then apk --no-cache del glib-dev;                fi; \
if [[ $__DISABLE_IPSET__    -eq 0 ]]; then apk --no-cache del ipset-dev;               fi; \
if [[ $__DISABLE_IPTABLES__ -eq 0 ]]; then apk --no-cache del iptables-dev;            fi; \
if [[ $__DISABLE_NFTABLES__ -eq 0 ]]; then apk --no-cache del libmnl-dev libnftnl-dev; fi; \
if [[ $__ENABLE_SNMP_VRRP__ -eq 1 ]]; then apk --no-cache del net-snmp-dev;            fi; \
if [[ $__ENABLE_REGEX__     -eq 1 ]]; then apk --no-cache del pcre2-dev;               fi; \
\
rm -rf $WDIR; \
\
addgroup -S keepalived_script && adduser -D -S -G keepalived_script keepalived_script

ENTRYPOINT ["/usr/local/bin/keepalived-wrapper","--dont-fork","--log-console", "-f", "/etc/keepalived/keepalived.conf"]
