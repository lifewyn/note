

![[Pasted image 20250407142112.png]]


>>> libmodbus 3.1.4 Downloading
--2025-04-07 14:20:15--  http://libmodbus.org/releases/libmodbus-3.1.4.tar.gz
Resolving libmodbus.org (libmodbus.org)... 172.67.204.233, 104.21.69.55, 2606:4700:3035::ac43:cce9, ...
Connecting to libmodbus.org (libmodbus.org)|172.67.204.233|:80... connected.
HTTP request sent, awaiting response... 301 Moved Permanently
Location: https://libmodbus.org/releases/libmodbus-3.1.4.tar.gz [following]
--2025-04-07 14:20:17--  https://libmodbus.org/releases/libmodbus-3.1.4.tar.gz
Connecting to libmodbus.org (libmodbus.org)|172.67.204.233|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 471576 (461K) [application/gzip]
Saving to: '/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/build/.libmodbus-3.1.4.tar.gz.3xuI9k/output'

/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/bui 100%[==========================================================================================================>] 460.52K   274KB/s    in 1.7s

2025-04-07 14:20:20 (274 KB/s) - '/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/build/.libmodbus-3.1.4.tar.gz.3xuI9k/output' saved [471576/471576]

libmodbus-3.1.4.tar.gz: OK (sha256: c8c862b0e9a7ba699a49bc98f62bdffdfafd53a5716c0e162696b4bf108d3637)
>>> libmodbus 3.1.4 Extracting
gzip -d -c /opt/TQT113_linux_V2.0/buildroot/buildroot-201902/dl/libmodbus/libmodbus-3.1.4.tar.gz | /opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/tar --strip-components=1 -C /opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/build/libmodbus-3.1.4   -xf -
>>> libmodbus 3.1.4 Patching
>>> libmodbus 3.1.4 Updating config.sub and config.guess
for file in config.guess config.sub; do for i in $(find /opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/build/libmodbus-3.1.4 -name $file); do cp support/gnuconfig/$file $i; done; done
>>> libmodbus 3.1.4 Patching libtool
patching file /opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/build/libmodbus-3.1.4/build-aux/ltmain.sh
Hunk #1 succeeded at 2694 (offset 7 lines).
Hunk #2 succeeded at 4284 (offset 7 lines).
Hunk #3 succeeded at 6579 (offset 25 lines).
Hunk #4 succeeded at 6589 (offset 25 lines).
Hunk #5 succeeded at 6882 (offset 25 lines).
Hunk #6 succeeded at 7174 (offset 25 lines).
Hunk #7 succeeded at 8140 (offset 28 lines).
Hunk #8 succeeded at 10769 (offset 59 lines).
>>> libmodbus 3.1.4 Configuring
(cd /opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/build/libmodbus-3.1.4/ && rm -rf config.cache && PATH="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin:/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/sbin:/opt/TQT113_linux_V2.0/prebuilt/hostbuilt/linux-x86/bin:/opt/TQT113_linux_V2.0/prebuilt/hostbuilt/linux-x86/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/opt/TQT113_linux_V2.0/build/bin:/opt/TQT113_linux_V2.0/build/bin" AR="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/arm-linux-gnueabi-ar" AS="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/arm-linux-gnueabi-as" LD="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/arm-linux-gnueabi-ld" NM="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/arm-linux-gnueabi-nm" CC="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/arm-linux-gnueabi-gcc" GCC="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/arm-linux-gnueabi-gcc" CPP="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/arm-linux-gnueabi-cpp" CXX="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/arm-linux-gnueabi-g++" FC="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/arm-linux-gnueabi-gfortran" F77="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/arm-linux-gnueabi-gfortran" RANLIB="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/arm-linux-gnueabi-ranlib" READELF="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/arm-linux-gnueabi-readelf" STRIP="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/arm-linux-gnueabi-strip" OBJCOPY="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/arm-linux-gnueabi-objcopy" OBJDUMP="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/arm-linux-gnueabi-objdump" AR_FOR_BUILD="/usr/bin/ar" AS_FOR_BUILD="/usr/bin/as" CC_FOR_BUILD="/usr/bin/gcc" GCC_FOR_BUILD="/usr/bin/gcc" CXX_FOR_BUILD="/usr/bin/g++" LD_FOR_BUILD="/usr/bin/ld" CPPFLAGS_FOR_BUILD="-I/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/include" CFLAGS_FOR_BUILD="-O2 -I/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/include" CXXFLAGS_FOR_BUILD="-O2 -I/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/include" LDFLAGS_FOR_BUILD="-L/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/lib -Wl,-rpath,/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/lib" FCFLAGS_FOR_BUILD="" DEFAULT_ASSEMBLER="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/arm-linux-gnueabi-as" DEFAULT_LINKER="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/arm-linux-gnueabi-ld" CPPFLAGS="-D_LARGEFILE_SOURCE -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64" CFLAGS="-D_LARGEFILE_SOURCE -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64  -Os  " CXXFLAGS="-D_LARGEFILE_SOURCE -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64  -Os  " LDFLAGS="" FCFLAGS=" -Os " FFLAGS=" -Os " PKG_CONFIG="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/pkg-config" STAGING_DIR="/opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/arm-buildroot-linux-gnueabi/sysroot" INTLTOOL_PERL=/usr/bin/perl ac_cv_lbl_unaligned_fail=yes ac_cv_func_mmap_fixed_mapped=yes ac_cv_func_memcmp_working=yes ac_cv_have_decl_malloc=yes gl_cv_func_malloc_0_nonnull=yes ac_cv_func_malloc_0_nonnull=yes ac_cv_func_calloc_0_nonnull=yes ac_cv_func_realloc_0_nonnull=yes lt_cv_sys_lib_search_path_spec="" ac_cv_c_bigendian=no   CONFIG_SITE=/dev/null ./configure --target=arm-buildroot-linux-gnueabi --host=arm-buildroot-linux-gnueabi --build=x86_64-pc-linux-gnu --prefix=/usr --exec-prefix=/usr --sysconfdir=/etc --localstatedir=/var --program-prefix="" --disable-gtk-doc --disable-gtk-doc-html --disable-doc --disable-docs --disable-documentation --with-xmlto=no --with-fop=no --disable-dependency-tracking --enable-ipv6 --disable-nls --disable-static --enable-shared  --without-documentation --disable-tests )
configure: WARNING: unrecognized options: --disable-gtk-doc, --disable-gtk-doc-html, --disable-doc, --disable-docs, --disable-documentation, --with-xmlto, --with-fop, --enable-ipv6, --disable-nls
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for arm-buildroot-linux-gnueabi-strip... /opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/arm-linux-gnueabi-strip
checking for a thread-safe mkdir -p... /usr/bin/mkdir -p
checking for gawk... gawk
checking whether make sets $(MAKE)... yes
checking whether make supports nested variables... yes
checking how to create a pax tar archive... gnutar
checking for style of include used by make... GNU
checking for arm-buildroot-linux-gnueabi-gcc... /opt/TQT113_linux_V2.0/out/t113_s4/evb1_auto/buildroot/buildroot/host/bin/arm-linux-gnueabi-gcc
