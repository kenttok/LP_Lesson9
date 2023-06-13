MACHINES = {
  :systemd => {
        :box_name => "centos/7",
	:box_version => "1804.02",
        },
   }
   
Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|
	config.vm.box_version = "1804.02"
	config.vm.define boxname do |box|
         box.vm.box = boxconfig[:box_name]
         box.vm.host_name = boxname.to_s
         box.vm.network "private_network", ip: boxconfig[:ip_addr]
         box.vm.provider :virtualbox do |vb|
           	  vb.customize ["modifyvm", :id, "--memory", "1024"]
end
	 box.vm.provision "shell", inline: <<-SHELL
	      mkdir -p ~root/.ssh
              cp ~vagrant/.ssh/auth* ~root/.ssh
	
yum install -y httpd
cp /usr/lib/systemd/system/httpd.service /etc/systemd/system/httpd@.service
sed -i.orig s@sysconfig/httpd@sysconfig/httpd-%I@ /etc/systemd/system/httpd@.service
cp /etc/sysconfig/httpd /etc/sysconfig/httpd-first
cp /etc/sysconfig/httpd /etc/sysconfig/httpd-second
sed -i.orig 's@^#OPTIONS=@OPTIONS="-f conf/first.conf"@' /etc/sysconfig/httpd-first
sed -i.orig 's@^#OPTIONS=@OPTIONS="-f conf/second.conf"@' /etc/sysconfig/httpd-second
cp /etc/httpd/conf/httpd.conf /etc/httpd/conf/first.conf
cp /etc/httpd/conf/httpd.conf /etc/httpd/conf/second.conf
sed -i.orig "s/^Listen 80/Listen 8080/" /etc/httpd/conf/first.conf
echo 'PidFile /var/run/httpd-first.pid' >> /etc/httpd/conf/first.conf
sed -i.orig "s/^Listen 80/Listen 8081/" /etc/httpd/conf/second.conf
echo 'PidFile /var/run/httpd-second.pid' >> /etc/httpd/conf/second.conf
systemctl start httpd@second
systemctl start httpd@first
        SHELL
	 end
	end
end
