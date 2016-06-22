FROM maven:3.3.9-jdk-8
MAINTAINER Xavier Calland <xca@atolcd.com>

# Inspired by https://hub.docker.com/r/gfx2015/android

# Install Deps
RUN dpkg --add-architecture i386 && \
  apt-get update && \
  apt-get install -y --force-yes expect \
    git \
    wget \
    libc6-i386 \
    lib32stdc++6 \
    lib32gcc1 \
    lib32ncurses5 \
    lib32z1 \
    python \
    curl

# Install SDK
ENV ANDROID_SDK_VERIONS 24.4.1
RUN cd /opt && \
  wget --output-document=android-sdk.tgz http://dl.google.com/android/android-sdk_r${ANDROID_SDK_VERIONS}-linux.tgz && \
  tar xzf android-sdk.tgz && \
  rm -f android-sdk.tgz

# Setup environment
ENV ANDROID_HOME /opt/android-sdk-linux
ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools

# Install sdk elements
ENV ANDROID_COMPONENTS platform-tools,build-tools-23.0.2,build-tools-23.0.3,android-23,android-19
RUN echo y | android update sdk --no-ui --all --filter "${ANDROID_COMPONENTS}"

RUN chown -R root: ${ANDROID_HOME} && chmod 777 -R ${ANDROID_HOME}

RUN which adb
RUN which android

# Cleaning
RUN apt-get clean

# GO to workspace
RUN mkdir -p /opt/workspace
WORKDIR /opt/workspace

# Support Gradle
ENV TERM dumb
