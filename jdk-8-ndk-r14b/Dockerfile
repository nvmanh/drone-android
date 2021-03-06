FROM java:openjdk-8-jdk

MAINTAINER haipq7641@gmail.com

VOLUME ["/opt/android"]

# Setup ENV
ENV ANDROID_NDK_VERSION r14b
ENV ANDROID_HOME /opt/android/sdk
ENV ANDROID_NDK_HOME /opt/android/ndk-${ANDROID_NDK_VERSION}
ENV ANDROID_NDK ${ANDROID_NDK_HOME}
ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools


# ------------------------------------------------------
# --- Install required tools

#RUN apt-get update -qq
RUN apt-get update


# ------------------------------------------------------

# download
RUN mkdir /opt/android-ndk-tmp
RUN cd /opt/android-ndk-tmp && wget -q https://dl.google.com/android/repository/android-ndk-${ANDROID_NDK_VERSION}-linux-x86_64.zip
# uncompress
RUN cd /opt/android-ndk-tmp && unzip -q android-ndk-${ANDROID_NDK_VERSION}-linux-x86_64.zip
# move to its final location
RUN cd /opt/android-ndk-tmp && mv ./android-ndk-${ANDROID_NDK_VERSION} ${ANDROID_NDK_HOME}
# remove temp dir
RUN rm -rf /opt/android-ndk-tmp
# add to PATH
ENV PATH ${PATH}:${ANDROID_NDK_HOME}


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
#ENV ANDROID_HOME /home/likewise-open/FRAMGIA/nguyen.viet.manh/Android/Sdk
ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools


RUN cd /opt && \
    wget -q ${ANDROID_SDK_URL} && \
    tar -xzf ${ANDROID_SDK_FILENAME} && \
    rm ${ANDROID_SDK_FILENAME}

#RUN cd /home/likewise-open/FRAMGIA/nguyen.viet.manh/Android && \
#    wget -q ${ANDROID_SDK_URL} && \
#    tar -xzf ${ANDROID_SDK_FILENAME} && \
#    rm ${ANDROID_SDK_FILENAME}

# Update Android SDK
RUN ( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | android update sdk -u -a --filter tools && \
    ( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | android update sdk -u -a --filter platform-tools && \
    ( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | android update sdk -u -a --filter extra-android-support && \
    ( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | android update sdk -u -a --filter extra-android-m2repository && \
    ( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | android update sdk -u -a --filter extra-google-google_play_services && \
    ( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | android update sdk -u -a --filter extra-google-m2repository 

RUN ( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | android update sdk -u -a --filter ${ANDROID_API_LEVELS} && \
    ( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | android update sdk -u -a --filter build-tools-23.0.3 && \
    ( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | android update sdk -u -a --filter build-tools-23.0.2 && \
    ( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | android update sdk -u -a --filter build-tools-23.0.1 && \
    ( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | android update sdk -u -a --filter build-tools-23.0.0 && \
    ( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | android update sdk -u -a --filter build-tools-24.0.1 && \
    ( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | android update sdk -u -a --filter build-tools-24.0.2 && \
    ( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | android update sdk -u -a --filter build-tools-24.0.3 && \
    ( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | android update sdk -u -a --filter build-tools-25.0.0 && \
    ( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | android update sdk -u -a --filter build-tools-25.0.1 && \
    ( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | android update sdk -u -a --filter build-tools-25.0.2 

# Update Android licenses
COPY ../licenses /opt/android-sdk-linux/licenses
#COPY /home/likewise-open/FRAMGIA/nguyen.viet.manh/Android/Sdk/licenses

# Install Framgia CLI
RUN curl -o /usr/bin/framgia-ci https://raw.githubusercontent.com/framgia/ci-report-tool/master/dist/framgia-ci && \
    chmod +x /usr/bin/framgia-ci

# Install Gradle
ENV GRADLE_VERSION 3.2.1

RUN cd /usr/bin && \
    wget -q https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-all.zip && \
    unzip gradle-${GRADLE_VERSION}-all.zip && \
    ln -s gradle-${GRADLE_VERSION} gradle && \
    rm gradle-${GRADLE_VERSION}-all.zip

ENV GRADLE_HOME /usr/bin/gradle
ENV PATH $PATH:$GRADLE_HOME/bin

# ------------------------------------------------------
# --- Cleanup and rev num

# Cleaning
RUN apt-get clean

# Install libstdc++
RUN apk update && apk add bash libstdc++ && rm -rf /var/cache/apk/*
