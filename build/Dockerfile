FROM archlinux:latest
WORKDIR /config
USER root
RUN mkdir /init2 && mkdir /system
COPY . /init2/

ARG EMBY_V=4.9.0.60
ARG EMBY_CRACK_V=4.9.0.60b
ARG FFMPEG_V=2023_06_25

ENV TZ=Asia/Shanghai

ENV FFDETECT=/usr/bin/ffdetect
ENV FFMPEG=/usr/bin/ffmpeg
ENV FFPROBE=/usr/bin/ffprobe
ENV PROGRAM_DATA=/config
ENV LIBVA_DRIVERS_PATH=/usr/lib/dri
ENV LD_LIBRARY_PATH=/usr/lib

ADD https://github.com/just-containers/s6-overlay/releases/download/v2.2.0.3/s6-overlay-amd64-installer /tmp/
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    chmod +x /tmp/s6-overlay-amd64-installer && /tmp/s6-overlay-amd64-installer / && \
    mkdir -p /etc/services.d/emby-server-s6/ && cp /init2/run /etc/services.d/emby-server-s6 && cp /init2/finish /etc/services.d/emby-server-s6 && chmod u+x /etc/services.d/emby-server-s6/* && \
    pacman -Syu --noconfirm && pacman -Sy --noconfirm alsa-lib aom aspnet-runtime-8.0 bzip2 dotnet-runtime-8.0 expat fontconfig fribidi glibc gmp gnutls lame libass chromaprint dav1d libdrm \
    freetype2 libjpeg-turbo libmfx libpng tesseract libtheora libva libvips libvorbis libwebp x264 x265 ocl-icd opus skia-sharp sqlite zvbi zlib intel-compute-runtime intel-media-driver intel-media-sdk \
    onevpl-intel-gpu libvpl libva-utils wget unzip nano lshw clinfo handbrake ffmpeg && \
    wget https://github.com/MediaBrowser/Emby.Releases/releases/download/${EMBY_V}/embyserver-netcore_${EMBY_V}.zip && unzip -d / embyserver-netcore_${EMBY_V}.zip && chmod -R u+x /system && \
    wget https://mediabrowser.github.io/embytools/ffmpeg-${FFMPEG_V}-x64.tar.xz && mkdir /ffmpeg_temp && tar xvf ffmpeg-${FFMPEG_V}-x64.tar.xz -C /ffmpeg_temp && mv /ffmpeg_temp/bin/ffdetect /bin/ && mv /ffmpeg_temp/lib/* /lib  && \
    wget https://cf.mb6.top/tmp/emby/${EMBY_CRACK_V}/docker_crack_${EMBY_CRACK_V}.sh && chmod 777 docker_crack_${EMBY_CRACK_V}.sh && ./docker_crack_${EMBY_CRACK_V}.sh && \
    rm -rf embyserver-netcore_${EMBY_V}.zip ffmpeg-${FFMPEG_V}-x64.tar.xz ffmpeg-master-latest-linux64-gpl.tar.xz /ffmpeg_temp && \
    pacman -Scc --noconfirm

ENTRYPOINT ["/init"]
# docker build --progress=plain -t linvery/emby_intelgpu:1.0 .