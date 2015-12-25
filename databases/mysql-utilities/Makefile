# Created by: KIKUCHI Koichiro <koichiro@rworks.jp>
# $FreeBSD$

PORTNAME=	mysql-utilities
PORTVERSION=	1.5.6
CATEGORIES=	databases
MASTER_SITES=	MYSQL/MySQLGUITools

MAINTAINER=	koichiro@rworks.jp
COMMENT=	MySQL management utilities

LICENSE=	GPLv2

NO_ARCH=	yes

USES=		python:2 shebangfix
USE_PYTHON=	distutils autoplist

SHEBANG_FILES=	scripts/*.py

SAMPLE_FILES=	etc/mysql/fabric.cfg
POST_PLIST+=	process-sample-files

.include <bsd.port.options.mk>

post-patch:
	@${REINPLACE_CMD} -e "s!'/etc'!'${PREFIX}/etc'!" ${WRKSRC}/setup.py

process-sample-files:
.for f in ${SAMPLE_FILES}
	@f=${STAGEDIR}${"${f:M/*}" != "":?:${PREFIX}/}${f};${MV} $$f $$f.sample
	@${REINPLACE_CMD} -e "s|^${f}$$|@sample ${f}.sample|" ${TMPPLIST}
.endfor

.include <bsd.port.pre.mk>

# PYDISTUTILS_INSTALLNOSINGLE is deprecated so...
PYDISTUTILS_INSTALLARGS:=	${PYDISTUTILS_INSTALLARGS:N--single-version-externally-managed}

.include <bsd.port.post.mk>
