FROM ubuntu
RUN apt-get update && apt-get install -y wget && apt-get install -y unzip
RUN wget https://github.com/containers/gvisor-tap-vsock/archive/refs/heads/main.zip
RUN unzip main.zip
COPY --from=golang:1.16 /usr/local/go/ /usr/local/go/
ENV PATH="/usr/local/go/bin:${PATH}"
RUN cd gvisor-tap-vsock-main
RUN apt-get install -y make
RUN cd /gvisor-tap-vsock-main && make
RUN apt-get update
RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
RUN apt-get install -y isc-dhcp-client
RUN apt-get install -y rng-tools
RUN mkdir -p /root/.ssh && echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCgyjEw2zSPx593LJfU40vrmXZQZLtgvjh3NKPD995r+zfX1joud/H5TzQkjq+03YSHBiCAzkklH5abMy29Y3UiZrsBDS0UN7buojx7TI0wi8H+ZlF1d3XfJG0BY2V2BqRo6oBTMfteU5gb8PwfvL/L7aKu8t1lnZN+QVG2YTr8MOw71p8RApkt7umbUxqO4ulr36EnY3fZIfJRImdzLmepgfd2ZLqv2SxexNshezJU19CDPvRakBEFa6bm7K5F3NKHwR3WT2vgCB8G/Z/APZk5IwfHEJOUsTo7rSJ9kvMQQX8RmkOHL14josKBaMLIbbDDr9IyiiOwie/AQcWIomJZ" >> /root/.ssh/authorized_keys
COPY run.sh /run.sh
COPY requirements.txt /requirements.txt

RUN apt-get install python3 -y
RUN apt-get install pip -y

RUN pip install --no-cache-dir --upgrade pip
RUN pip install -r requirements.txt

CMD ["./run.sh"]
