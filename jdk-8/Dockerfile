FROM java:openjdk-8-alpine

MAINTAINER haipq7641@gmail.com

# Setup ENV
ENV ANDROID_HOME /opt/android/sdk
ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools

# Install libstdc++
RUN apk update && apk add bash libstdc++ && rm -rf /var/cache/apk/*

# Installs Android SDK
ENV ANDROID_SDK_FILENAME android-sdk_r24.4.1-linux.tgz
ENV ANDROID_SDK_URL http://dl.google.com/android/${ANDROID_SDK_FILENAME}

ENV ANDROID_API_LEVELS android-23,android-24,android-25  

ENV ANDROID_HOME /opt/android-sdk-linux
ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools

RUN apk add --no-cache curl ca-certificates bash && \
    mkdir -p /opt && curl -L ${ANDROID_SDK_URL} | tar zxv -C /opt && \

# Update Android SDK
RUN echo "y" | android update sdk -u -a --filter tools && \
    echo "y" | android update sdk -u -a --filter platform-tools && \
    echo "y" | android update sdk -u -a --filter extra-android-support && \
    echo "y" | android update sdk -u -a --filter extra-android-m2repository && \
    echo "y" | android update sdk -u -a --filter extra-google-google_play_services && \
    echo "y" | android update sdk -u -a --filter extra-google-m2repository 

RUN echo "y" | android update sdk -u -a --filter ${ANDROID_API_LEVELS} && \
    echo "y" | android update sdk -u -a --filter build-tools-23.0.2 && \
    echo "y" | android update sdk -u -a --filter build-tools-23.0.1 && \
    echo "y" | android update sdk -u -a --filter build-tools-23.0.0 && \
    echo "y" | android update sdk -u -a --filter build-tools-24.0.1 && \
    echo "y" | android update sdk -u -a --filter build-tools-24.0.2 && \
    echo "y" | android update sdk -u -a --filter build-tools-24.0.3 && \
    echo "y" | android update sdk -u -a --filter build-tools-25 && \
    echo "y" | android update sdk -u -a --filter build-tools-25.0.1

# Install Gradle
RUN mkdir /usr/lib/gradle /app
ENV GRADLE_VERSION 3.2.1
ENV GRADLE_HOME /usr/lib/gradle/gradle-${GRADLE_VERSION}
ENV PATH ${PATH}:${GRADLE_HOME}/bin

WORKDIR /usr/lib/gradle

RUN set -x \
  && apk add --no-cache wget \
  && wget https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip \
  && unzip gradle-${GRADLE_VERSION}-bin.zip \
  && rm gradle-${GRADLE_VERSION}-bin.zip \
  && apk del wget

RUN apk update && apk add bash libstdc++ && rm -rf /var/cache/apk/*