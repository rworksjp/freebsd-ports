# $FreeBSD$

PORTNAME=	glusterfs
PORTVERSION=	3.7.6
PORTREVISION=	1
CATEGORIES=	sysutils net
MASTER_SITES=	http://bits.gluster.org/pub/gluster/glusterfs/src/
DISTNAME=	${PORTNAME}-${PORTVERSION}

MAINTAINER=	craig001@lerwick.hopto.org
COMMENT=	GlusterFS distributed file system

LICENSE=	GPLv2 LGPL3
LICENSE_COMB=	dual

LIB_DEPENDS=	liburcu-bp.so:${PORTSDIR}/sysutils/liburcu

USES=		bison gettext libtool pkgconfig python:build readline \
		shebangfix
SHEBANG_FILES=	extras/peer_add_secret_pub.in \
		tools/glusterfind/S57glusterfind-delete-post.py \
		extras/ganesha/scripts/ganesha-ha.sh
USE_GNOME=	libxml2
USE_LDCONFIG=	yes
USE_OPENSSL=	yes
USE_SQLITE=	yes
USE_RC_SUBR=	glusterd
GNU_CONFIGURE=	yes
CONFIGURE_ARGS=	--with-pkgconfigdir=${PREFIX}/libdata/pkgconfig \
		--with-mountutildir=${PREFIX}/sbin \
		--localstatedir=/var \
		--disable-epoll \
		--enable-glupy \
		--disable-georeplication \
		ac_cv_lib_aio_io_setup=no ac_cv_func_fallocate=no \
		ac_cv_func_setfsuid=no 	ac_cv_func_fdatasync=no \
		ac_cv_func_llistxattr=no ac_cv_func_malloc_stats=no
INSTALL_TARGET=	install-strip
PLIST_SUB=	GLUSTERLIBDIR="lib/${PORTNAME}/${PORTVERSION}"
KMODDIR?=	/boot/modules

OPTIONS_DEFINE=	ARGP QEMU
OPTIONS_DEFAULT=QEMU
ARGP_DESC=	Use devel/argp-standalone instead of bundled one
QEMU_DESC=	QEMU Block formats support

.include <bsd.port.options.mk>

.if ${OSVERSION} < 1000000
WITH_OPENSSL_PORT=	yes
RUN_DEPENDS+=	${KMODDIR}/fuse.ko:${PORTSDIR}/sysutils/fusefs-kmod
.endif

.if ${PORT_OPTIONS:MARGP}
LIB_DEPENDS+=	libargp.so:${PORTSDIR}/devel/argp-standalone 
CPPFLAGS+=	-I"${LOCALBASE}/include"
LDFLAGS+=	-L"${LOCALBASE}/lib" -largp
.endif

.if ${PORT_OPTIONS:MQEMU}
USE_GNOME+=	glib20
CONFIGURE_ARGS+=	--enable-qemu-block
PLIST_SUB+=	QEMU=""
.else
CONFIGURE_ARGS+=	--disable-qemu-block
PLIST_SUB+=	QEMU="@comment "
.endif

post-install:
	@${LN} -sf ${PREFIX}/sbin/mount_glusterfs /usr/sbin

.include <bsd.port.mk>
