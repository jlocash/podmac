$script = <<-SHELL
yum install -y podman

groupadd -f -r podman

#systemctl edit podman.socket
mkdir -p /etc/systemd/system/podman.socket.d
cat >/etc/systemd/system/podman.socket.d/override.conf <<EOF
[Socket]
SocketMode=0660
SocketUser=root
SocketGroup=podman
EOF
systemctl daemon-reload
echo "d /run/podman 0770 root podman" > /etc/tmpfiles.d/podman.conf
sudo systemd-tmpfiles --create

systemctl enable podman.socket
systemctl start podman.socket

usermod -aG podman $SUDO_USER
SHELL
Vagrant.configure("2") do |config|
    config.vm.box = "fedora/32-cloud-base"
    config.vm.provider "virtualbox" do |vb|
        vb.memory = "1024"
    end
    config.vm.provision "shell", inline:$script
end

