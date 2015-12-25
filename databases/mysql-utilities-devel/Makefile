# Created by: KIKUCHI Koichiro <koichiro@rworks.jp>
# $FreeBSD$

PORTNAME=	mysql-utilities
PORTVERSION=	1.6.2
CATEGORIES=	databases
MASTER_SITES=	MYSQL/MySQLGUITools
PKGNAMESUFFIX=	-devel

MAINTAINER=	koichiro@rworks.jp
COMMENT=	MySQL management utilities

LICENSE=	GPLv2

USES=		python:2 shebangfix
USE_PYTHON=	distutils autoplist

SHEBANG_FILES=	scripts/*.py

.include <bsd.port.options.mk>

post-patch:
	@${REINPLACE_CMD} -e "s!'/etc'!'${PREFIX}/etc'!" ${WRKSRC}/setup.py

.include <bsd.port.pre.mk>

# PYDISTUTILS_INSTALLNOSINGLE is deprecated so...
PYDISTUTILS_INSTALLARGS:=	${PYDISTUTILS_INSTALLARGS:N--single-version-externally-managed}

.include <bsd.port.post.mk>
