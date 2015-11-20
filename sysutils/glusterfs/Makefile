# $FreeBSD$

PORTNAME=	glusterfs
PORTVERSION=	3.7.6
CATEGORIES=	sysutils net
MASTER_SITES=	http://bits.gluster.org/pub/gluster/glusterfs/src/
DISTNAME=	${PORTNAME}-${PORTVERSION}

MAINTAINER=	craig001@lerwick.hopto.org
COMMENT=	GlusterFS distributed file system

LICENSE=	GPLv2 LGPL3
LICENSE_COMB=	dual

LIB_DEPENDS=	libargp.so:${PORTSDIR}/devel/argp-standalone liburcu-bp.so:${PORTSDIR}/sysutils/liburcu

USE_GCC=	any

USES=		bison gettext libtool pkgconfig python:build readline \
		shebangfix
SHEBANG_FILES=	extras/peer_add_secret_pub.in \
		tools/glusterfind/S57glusterfind-delete-post.py \
		extras/ganesha/scripts/ganesha-ha.sh
USE_GNOME=	glib20 libxml2
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
CPPFLAGS+=	-I"${LOCALBASE}/include"
LDFLAGS+=	-L"${LOCALBASE}/lib" -largp
INSTALL_TARGET=	install-strip
PLIST_SUB=	GLUSTERLIBDIR="${PREFIX}/lib/${PORTNAME}/${PORTVERSION}"

.include <bsd.port.mk>
