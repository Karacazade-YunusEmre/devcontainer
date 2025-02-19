FROM ubuntu:24.04

# Prerequisites
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y curl git unzip xz-utils zip libglu1-mesa wget\
                       clang cmake ninja-build pkg-config libgtk-3-dev liblzma-dev

# Setup new user
RUN useradd -ms /bin/bash developer
USER developer
WORKDIR /home/developer

# Download and Setup Java
RUN wget -O jdk.tar.gz https://download.oracle.com/java/17/archive/jdk-17.0.12_linux-x64_bin.tar.gz
RUN tar -xf jdk.tar.gz && rm jdk.tar.gz
RUN mv jdk* jdk

# Download and Setup NodeJs
RUN wget -O node.tar.xz https://nodejs.org/dist/v22.14.0/node-v22.14.0-linux-x64.tar.xz
RUN tar -xf node.tar.xz && rm node.tar.xz
RUN mv node* node

# Setup Environment
ENV HOME /home/developer
ENV JAVA_HOME ${HOME}/jdk
ENV PATH $PATH:${JAVA_HOME}"/bin"
ENV ANDROID_HOME ${HOME}/Android/sdk
ENV PATH $PATH:${ANDROID_HOME}"/cmdline-tools"
ENV PATH $PATH:${ANDROID_HOME}"/cmdline-tools/tools"
ENV PATH $PATH:${ANDROID_HOME}"/cmdline-tools/tools/bin" 
ENV PATH $PATH:${ANDROID_HOME}"/platform-tools"
ENV PATH $PATH:${HOME}"/node/bin"
ENV ANDROID_SDK_ROOT ${ANDROID_HOME}
ENV PATH "$PATH:${HOME}/flutter/bin"
ENV PATH "$PATH:${HOME}/.pub-cache/bin"

# Prepare Android directories and system variables
RUN mkdir -p Android/sdk
RUN mkdir -p Android/sdk/cmdline-tools
RUN mkdir -p .android && touch .android/repositories.cfg

# Set up Android SDK
RUN wget -O commandlinetools.zip https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip?hl=tr
RUN wget -O platform-tools.zip https://dl.google.com/android/repository/platform-tools-latest-linux.zip
RUN unzip commandlinetools.zip && rm commandlinetools.zip
RUN unzip platform-tools.zip && rm platform-tools.zip
RUN mv cmdline-tools tools
RUN mv tools Android/sdk/cmdline-tools/
RUN mv platform-tools Android/sdk/

RUN yes | sdkmanager --licenses
RUN sdkmanager "build-tools;35.0.0" 
RUN sdkmanager "platforms;android-35" 
RUN sdkmanager "platform-tools" 
RUN sdkmanager "cmdline-tools;latest"
RUN sdkmanager "sources;android-35"
RUN sdkmanager --update

# Download Flutter SDK
RUN wget -O flutter.tar.xz https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_3.29.0-stable.tar.xz
RUN tar -xf flutter.tar.xz && rm flutter.tar.xz
RUN git config --global --add safe.directory /home/developer/flutter

# Copy Script file
COPY script.sh .
