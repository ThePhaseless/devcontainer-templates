FROM mcr.microsoft.com/devcontainers/base:debian

# Get latest Android SDK
ENV ANDROID_HOME=/opt/android-sdk-linux
ENV ANDROID_SDK_ROOT=$ANDROID_HOME
COPY --from=mobiledevops/android-sdk-image $ANDROID_HOME $ANDROID_HOME
RUN rm ${ANDROID_HOME}/cmdline-tools/* -r
RUN mkdir -p $ANDROID_HOME/cmdline-tools/latest
COPY --from=mobiledevops/android-sdk-image $ANDROID_HOME/cmdline-tools $ANDROID_HOME/cmdline-tools/latest

ENV PATH="${ANDROID_HOME}/cmdline-tools/latest/bin:${ANDROID_HOME}/platform-tools:${PATH}"

# Get latest Java SDK
ENV JAVA_HOME=/opt/java/openjdk
COPY --from=eclipse-temurin:latest $JAVA_HOME $JAVA_HOME
ENV PATH="${JAVA_HOME}/bin:${PATH}"

# Get latest Flutter
RUN mkdir -p /usr/bin/flutter
RUN chown 1000:1000 /usr/bin/flutter -R

USER vscode
RUN git clone -b stable https://github.com/flutter/flutter /usr/bin/flutter
RUN chmod +x /usr/bin/flutter/bin/flutter
ENV PATH="/usr/bin/flutter/bin:${PATH}"
RUN flutter precache

# Install flutter bash completion
RUN mkdir ~/.bash-completion -p
RUN flutter bash-completion > ~/.bash-completion/flutter
RUN chmod +x ~/.bash-completion/flutter

# Install linux dependencies
RUN sudo apt-get update
RUN sudo apt-get install -y clang cmake ninja-build pkg-config libgtk-3-dev

# Source flutter bash completion
RUN echo 'source ~/.bash-completion/flutter' >> ~/.bashrc
