
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/precise64"
  config.vm.synced_folder ".", "/vagrant", type: "smb"
  config.vm.provider "virtualbox" do |v|
          v.memory = 3192
          v.cpus = 2
        end

  config.vm.provision "shell", inline: $script
end

$script = <<SCRIPT
cd /opt
mkdir drilltest
cd drilltest
wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/7u72-b14/jdk-7u72-linux-x64.tar.gz"
wget http://getdrill.org/drill/download/apache-drill-0.7.0.tar.gz
cd /opt

tar zxvf /opt/drilltest/jdk-7u72-linux-x64.tar.gz
tar zxvf /opt/drilltest/apache-drill-0.7.0.tar.gz
export JAVA_HOME=/opt/jdk1.7.0_72
export PATH=$PATH:/opt/jdk1.7.0_72/bin
cd /opt/apache-drill-0.7.0
/opt/apache-drill-0.7.0/bin/drillbit.sh start

SCRIPT
