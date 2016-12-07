FROM java:openjdk-8-jdk

MAINTAINER haipq7641@gmail.com

# Installs i386 architecture required for running 32 bit Android tools
RUN dpkg --add-architecture i386 && \
    apt-get update -y && \
    apt-get install -y libc6:i386 libncurses5:i386 libstdc++6:i386 lib32z1 && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get autoremove -y && \
    apt-get clean
   
# Installs Android SDK
ENV ANDROID_SDK_FILENAME android-sdk_r24.4.1-linux.tgz
ENV ANDROID_SDK_URL http://dl.google.com/android/${ANDROID_SDK_FILENAME}
ENV ANDROID_API_LEVELS android-23,android-24,android-25  
ENV ANDROID_HOME /opt/android-sdk-linux
ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools

RUN cd /opt && \
    wget -q ${ANDROID_SDK_URL} && \
    tar -xzf ${ANDROID_SDK_FILENAME} && \
    rm ${ANDROID_SDK_FILENAME} && \

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
ARG GRADLE_VERSION=3.2.1
WORKDIR /usr/bin
RUN curl -sLO https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-all.zip && \
  unzip gradle-${GRADLE_VERSION}-all.zip && \
  ln -s gradle-${GRADLE_VERSION} gradle && \
  rm gradle-${GRADLE_VERSION}-all.zip

ENV GRADLE_HOME /usr/bin/gradle
ENV PATH $PATH:$GRADLE_HOME/bin

# Cleaning
RUN apt-get clean