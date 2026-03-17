FROM ubuntu:22.04

ARG DEBIAN_FRONTEND=noninteractive

# ── System packages ──────────────────────────────────────────────────
RUN apt-get update && apt-get install -y --no-install-recommends \
        curl unzip wget git sudo \
        openjdk-17-jdk-headless \
        # needed by sdkmanager / aapt2
        libstdc++6 zlib1g \
    && rm -rf /var/lib/apt/lists/*

# ── Create vscode user ──────────────────────────────────────────────
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m -s /bin/bash $USERNAME \
    && echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# ── Environment ──────────────────────────────────────────────────────
ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
ENV ANDROID_HOME=/opt/android-sdk
ENV ANDROID_SDK_ROOT=${ANDROID_HOME}
ENV PATH="${ANDROID_HOME}/cmdline-tools/latest/bin:${ANDROID_HOME}/platform-tools:${PATH}"

# ── Android command-line tools ───────────────────────────────────────
ARG CMDLINE_TOOLS_VERSION=13114758
RUN mkdir -p ${ANDROID_HOME}/cmdline-tools \
    && curl -fsSL "https://dl.google.com/android/repository/commandlinetools-linux-${CMDLINE_TOOLS_VERSION}_latest.zip" \
       -o /tmp/cmdline-tools.zip \
    && unzip -q /tmp/cmdline-tools.zip -d ${ANDROID_HOME}/cmdline-tools \
    && mv ${ANDROID_HOME}/cmdline-tools/cmdline-tools ${ANDROID_HOME}/cmdline-tools/latest \
    && rm /tmp/cmdline-tools.zip

# ── Accept licences & install SDK components ─────────────────────────
RUN yes | sdkmanager --licenses > /dev/null 2>&1 || true
RUN sdkmanager --update
RUN sdkmanager \
        "platform-tools" \
        "platforms;android-35" \
        "build-tools;35.0.0" \
        "ndk;27.0.12077973"

# ── Gradle caching ──────────────────────────────────────────────────
ENV GRADLE_USER_HOME=/home/${USERNAME}/.gradle
RUN mkdir -p ${GRADLE_USER_HOME} \
    && chown -R ${USERNAME}:${USERNAME} /home/${USERNAME}

USER ${USERNAME}
