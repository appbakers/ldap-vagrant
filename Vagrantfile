# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu1404"

  ldap_hostname = ENV['G_LDAP_HOSTNAME'] ||= "ldap.vagrant.dev";
  ldap_ip = ENV['G_LDAP_IP'] ||= "192.168.33.253";
  ldap_port = ENV['G_LDAP_PORT'] ||= "13890";
  config.vm.hostname = ldap_hostname;
  config.vm.network "private_network", ip: ldap_ip;

  config.vm.provider "virtualbox" do |vb|
    vb.linked_clone = true
    vb.memory = "1024"
  end


$scr_nix_preconfig = <<SCRIPTBLOCK
sed -i 's/us.archive.ubuntu.com/ftp.daum.net/g' /etc/apt/sources.list
sed -i 's/kr.archive.ubuntu.com/ftp.daum.net/g' /etc/apt/sources.list
sed -i 's/security.ubuntu.com/ftp.daum.net/g' /etc/apt/sources.list
sed -i 's/extras.ubuntu.com/ftp.daum.net/g' /etc/apt/sources.list
apt-get update -y
apt-get install -y wget
SCRIPTBLOCK
$scr_nix_tz_kst= <<SCRIPTBLOCK
echo `date`
if [ -f /usr/share/zoneinfo/Asia/Seoul  ];then ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime;
else echo "Please check timezone info. ( currently only ubuntu is implemented.  )"; return false; fi;
echo `date`
SCRIPTBLOCK

  config.vm.network "forwarded_port", guest: 389, host: ldap_port.to_i;
  config.vm.provision "shell", name:"scr_nix_preconfig", inline:$scr_nix_preconfig;
  config.vm.provision "shell", name:"scr_nix_tz_kst", inline:$scr_nix_tz_kst;
  config.vm.provision "shell", path: "provision.sh"
end
