# Created by: KIKUCHI Koichiro <koichiro@rworks.jp>
# $FreeBSD$

PORTNAME=	mysql-utilities
PORTVERSION=	1.5.6
CATEGORIES=	databases
MASTER_SITES=	MYSQL/MySQLGUITools

MAINTAINER=	koichiro@rworks.jp
COMMENT=	MySQL management utilities

LICENSE=	GPLv2

USES=		python:2 shebangfix
USE_PYTHON=	distutils autoplist

SHEBANG_FILES=	scripts/*.py

# Create a 6 integer version string.
MYSQLUTIL_REL=	${PORTVERSION:C/^([0-9]+\.[0-9]+\.[0-9]+).*/\1/:C/([0-9]+)/0\1/g:C/0?([0-9][0-9])/\1/g:S/.//g}
.if ${MYSQLUTIL_REL} < 010602
SAMPLE_FILES=	etc/mysql/fabric.cfg
POST_PLIST=	process-sample-files
.endif
.include <bsd.port.options.mk>

post-patch:
	@${REINPLACE_CMD} -e "s!'/etc'!'${PREFIX}/etc'!" ${WRKSRC}/setup.py

.if ${MYSQLUTIL_REL} < 010602
process-sample-files:
.	for _file in ${SAMPLE_FILES}
	@cd ${STAGEDIR}${PREFIX}; \
		if [ -e ${_file} ]; then \
			${MV} ${_file} ${_file}.sample; \
			${REINPLACE_CMD} -e 's|^\(${_file}\)$$|@sample \1.sample|' ${TMPPLIST}; \
		fi
.	endfor
.endif

.include <bsd.port.pre.mk>

# PYDISTUTILS_INSTALLNOSINGLE is deprecated so...
PYDISTUTILS_INSTALLARGS:=	${PYDISTUTILS_INSTALLARGS:N--single-version-externally-managed}

.include <bsd.port.post.mk>
