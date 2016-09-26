FROM python:3.5
MAINTAINER David Yip <yipdw@member.fsf.org>

RUN apt-get update -yqq && apt-get dist-upgrade -yqq
RUN apt-get install -yqq fontconfig

WORKDIR /tmp
RUN wget https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-2.1.1-linux-x86_64.tar.bz2
RUN echo '86dd9a4bf4aee45f1a84c9f61cf1947c1d6dce9b9e8d2a907105da7852460d2f  phantomjs-2.1.1-linux-x86_64.tar.bz2' > /tmp/sha256sums
RUN sha256sum -c sha256sums
RUN tar jxf phantomjs-2.1.1-linux-x86_64.tar.bz2
RUN mv /tmp/phantomjs-2.1.1-linux-x86_64/bin/phantomjs /usr/local/bin
RUN adduser --home /home/archivebot --shell /bin/bash --uid 1000 archivebot --quiet --disabled-password

WORKDIR /home/archivebot
RUN git clone https://github.com/ArchiveTeam/ArchiveBot
RUN cd ArchiveBot/pipeline && pip3 install -r requirements.txt
RUN chown -R archivebot:archivebot /home/archivebot/ArchiveBot
USER archivebot
ENV RSYNC_URL=rsync://fos.textfiles.com/archivebot/ \
	REDIS_URL=redis://redis:6379/0 \
	FINISHED_WARCS_DIR=/home/archivebot/warcs4fos \
	NO_SCREEN=1

WORKDIR /home/archivebot/ArchiveBot/pipeline
VOLUME /home/archivebot/ArchiveBot/pipeline/data
VOLUME /home/archivebot/warcs4fos
CMD /usr/local/bin/run-pipeline3 pipeline.py --disable-web-server ${PIPELINE_NAME}
