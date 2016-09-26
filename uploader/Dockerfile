FROM python:3.5
MAINTAINER David Yip <yipdw@member.fsf.org>

RUN apt-get update -yqq && apt-get dist-upgrade -yqq
RUN apt-get install -yqq rsync
RUN pip3 install requests
RUN adduser --home /home/archivebot --shell /bin/bash --uid 1000 archivebot --quiet --disabled-password

VOLUME /home/archivebot/warcs4fos

WORKDIR /home/archivebot
RUN git clone https://github.com/ArchiveTeam/ArchiveBot
USER archivebot
ENV RSYNC_URL=rsync://fos.textfiles.com/archivebot/ \
	FINISHED_WARCS_DIR=/home/archivebot/warcs4fos

WORKDIR /home/archivebot/ArchiveBot/uploader
CMD python3 uploader.py
