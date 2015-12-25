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

.include <bsd.port.pre.mk>

# PYDISTUTILS_INSTALLNOSINGLE is deprecated so...
PYDISTUTILS_INSTALLARGS:=	${PYDISTUTILS_INSTALLARGS:N--single-version-externally-managed}

process-sample-files:
.for _file in ${SAMPLE_FILES:C|^|${STAGEDIR}${PREFIX}/|}
	@${MV} ${_file} ${_file}.sample
	@${REINPLACE_CMD} -e 's|^\(${_file}\)$$|@sample \1.sample|' ${TMPPLIST}
.endfor

.include <bsd.port.post.mk>
