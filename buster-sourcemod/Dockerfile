###########################################################
# Dockerfile that builds a CSGO Gameserver
###########################################################
FROM cm2network/steamcmd:root

LABEL maintainer="walentinlamonos@gmail.com"

ENV STEAMAPPID 740
ENV SERVERNO 1
ENV STEAMAPPDIR /hdd/docker-csgo/go_m${SERVERNO}

# Create autoupdate config
# Add entry script & ESL config
# Remove packages and tidy up
RUN set -x \
	&& apt-get update \
	&& apt-get install -y --no-install-recommends --no-install-suggests \
		wget=1.20.1-1.1 \
		ca-certificates=20190110 \
	&& mkdir -p ${STEAMAPPDIR}/csgo \
	&& cd ${STEAMAPPDIR} \
	&& wget https://raw.githubusercontent.com/gitgudpri3st/csgo-doc/master/etc/entry.sh \
	&& chmod 755 ${STEAMAPPDIR}/entry.sh \
	&& cd ${STEAMAPPDIR}/csgo \
	&& { \
			echo '@ShutdownOnFailedCommand 1'; \
			echo '@NoPromptForPassword 1'; \
			echo 'login anonymous'; \
			echo 'force_install_dir ${STEAMAPPDIR}'; \
			echo 'app_update ${STEAMAPPID}'; \
			echo 'quit'; \
		} > ${STEAMAPPDIR}/csgo_update.txt \
	&& wget -qO- https://raw.githubusercontent.com/gitgudpri3st/csgo-doc/master/etc/cfg.tar.gz | tar xvzf - \
	&& wget -qO- https://mms.alliedmods.net/mmsdrop/1.10/mmsource-1.10.7-git971-linux.tar.gz | tar xvzf - \
	&& wget -qO- https://sm.alliedmods.net/smdrop/1.10/sourcemod-1.10.0-git6461-linux.tar.gz | tar xvzf - \
	&& chown -R kaiser:kaiser ${STEAMAPPDIR} \
	&& apt-get remove --purge -y \
		wget \
	&& apt-get clean autoclean \
	&& apt-get autoremove -y \
	&& rm -rf /var/lib/apt/lists/*

ENV SRCDS_FPSMAX=300 \
	SRCDS_TICKRATE=128 \
	SRCDS_PORT=27020 \
	SRCDS_TV_PORT=27050 \
	SRCDS_CLIENT_PORT=27005 \
	SRCDS_MAXPLAYERS=16 \
	SRCDS_TOKEN=0 \
	SRCDS_RCONPW="changeme" \
	SRCDS_PW="changeme" \
	SRCDS_STARTMAP="de_dust2" \
	SRCDS_REGION=3 \
	SRCDS_MAPGROUP="mg_active" \
	SRCDS_GAMETYPE=0 \
	SRCDS_GAMEMODE=1

USER kaiser

WORKDIR $STEAMAPPDIR

VOLUME $STEAMAPPDIR

ENTRYPOINT ${STEAMAPPDIR}/entry.sh

# Expose ports
EXPOSE 27021/tcp 27021/udp 27051/udp
