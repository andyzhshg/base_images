# 使用官方的 Ubuntu 基础镜像
FROM ubuntu:22.04

# Set environment variables to avoid interactive prompts during installation
ENV DEBIAN_FRONTEND=noninteractive
ENV SSH_USERNAME=ubuntu
ENV PASSWORD=password

# Install OpenSSH server and clean up
RUN apt-get update \
    && apt-get install -y openssh-server iputils-ping telnet iproute2 sudo \
    && apt-get clean \
    && rm -rf /tmp/* /var/tmp/*

# 允许 sudo 组的所有用户无密码 sudo
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# Create the privilege separation directory and fix permissions
RUN mkdir -p /run/sshd \
    && chmod 755 /run/sshd

# Expose SSH port
EXPOSE 22

# Create the non-root user with the ability to set a password and authorized keys using environment variables
RUN useradd -ms /bin/bash $SSH_USERNAME

# Set up SSH configuration
RUN mkdir -p /home/$SSH_USERNAME/.ssh && chown $SSH_USERNAME:$SSH_USERNAME /home/$SSH_USERNAME/.ssh \
    && echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config \
    && echo "PermitRootLogin no" >> /etc/ssh/sshd_config

# Copy the script to configure the user's password and authorized keys
COPY entrypoint.sh /
RUN chmod +x /entrypoint.sh

# 使用自定义的 entrypoint 启动脚本
CMD ["/entrypoint.sh"]
