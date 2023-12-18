# Created by: MACIUS Matthew Kuzminski <mattkuzminski@gmail.com>
# $FreeBSD: head/multimedia/linux-xmovie/Makefile 340722 2014-01-22 17:00:46Z mat $

PORTNAME=	plexmediaserver
PORTVERSION=	0.9.11.4.739
DISTVERSION=	${PORTVERSION}-a4e710f
CATEGORIES=	multimedia linux
MASTER_SITES=	https://downloads.plex.tv/plex-media-server/0.9.11.4.739-a4e710f/
PKGNAMEPREFIX=	linux-
DISTNAME=	${PORTNAME}_${DISTVERSION}_i386.deb
EXTRACT_SUFX=   

MAINTAINER=	maciuszek@freebsdServ.local
COMMENT=	Backend for Plex media systems

ONLY_FOR_ARCHS=	i386
USE_LINUX=	yes
EXTRACT_CMD=	ar -x
EXTRACT_BEFORE_ARGS=
EXTRACT_AFTER_ARGS=
NO_BUILD=	yes
USE_RC_SUBR=	linux-plexmediaserver

WRKSRC=		${WRKDIR}/data-explode

USERS=  plex
GROUPS= plex

do-install:
	@${MKDIR} ${WRKSRC}
	@${MKDIR} ${STAGEDIR}/

	${TAR} -C ${WRKSRC} -xf ${WRKDIR}/data.tar.gz

	@${MKDIR} -p ${STAGEDIR}/${PREFIX}/etc/default
	${MV} ${WRKSRC}/etc/default/* ${STAGEDIR}/${PREFIX}/etc/default/

	${CHOWN} -R ${USERS}:${GROUPS} ${STAGEDIR}/

	@${MKDIR} -p ${STAGEDIR}/${PREFIX}/${PORTNAME}/bin/ ${STAGEDIR}/${PREFIX}/${PORTNAME}/lib/

	${MV} ${WRKSRC}/usr/lib/${PORTNAME}/* ${STAGEDIR}/${PREFIX}/${PORTNAME}/lib/
	${MV} ${WRKSRC}/usr/sbin/start_pms ${STAGEDIR}/${PREFIX}/${PORTNAME}/bin/

	${SED} -i -e "s#/usr/lib/plexmediaserver#${PREFIX}/${PORTNAME}/lib#g" ${STAGEDIR}/${PREFIX}/${PORTNAME}/bin/start_pms
	${SED} -i -e "s#/etc/default/plexmediaserver#${PREFIX}/etc/default/plexmediaserver#g" ${STAGEDIR}/${PREFIX}/${PORTNAME}/bin/start_pms	

	@${MKDIR} -p ${STAGEDIR}/${PREFIX}/bin
	${LN} -s ${PREFIX}/${PORTNAME}/bin/start_pms ${STAGEDIR}/${PREFIX}/bin/start_pms

	${MV} ${WRKSRC}/usr/share/applications/plexmediamanager.desktop ${STAGEDIR}/${PREFIX}/${PORTNAME}/
	${SED} -i -e "s/x-www-browser/xdg-open/" ${STAGEDIR}/${PREFIX}/${PORTNAME}/plexmediamanager.desktop

	@${MKDIR} -p ${STAGEDIR}/${PREFIX}/share/applications 
	${LN} -s ${PREFIX}/${PORTNAME}/plexmediamanager.desktop ${STAGEDIR}/${PREFIX}/share/applications/plexmediamanager.desktop

	@${MKDIR} -p ${STAGEDIR}/${PREFIX}/var/lib/${PORTNAME}
	${CHOWN} -R dlna:dlna ${STAGEDIR}/${PREFIX}/var/lib/${PORTNAME}

	@${BRANDELF} -t Linux ${STAGEDIR}/${PREFIX}/${PORTNAME}/lib/Plex*

.include <bsd.port.mk>`
