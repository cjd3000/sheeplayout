# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT
INSTALL_DIR=/opt/install/sheepdog
SHEEP=$INSTALL_DIR/sbin/sheep
DOG=$INSTALL_DIR/sbin/dog
STORE=/sheepstore
TAG=#{ENV.fetch('SHEEPDOG_TAG', 'master')}

# provision apt packages
apt-get update -qq
sudo apt-get install -y corosync libcorosync-dev liburcu-dev yasm \
  git make dh-autoreconf pkg-config qemu tree

# check out and install sheepdog
mkdir -p /opt/src
cd /opt/src
git clone https://github.com/sheepdog/sheepdog.git
cd sheepdog
git checkout $TAG
./autogen.sh
./configure --prefix=$INSTALL_DIR && make -j2 && make install

# make local cluster
mkdir $STORE
for i in 0 1 2; do
  $SHEEP -c local $STORE/$i -z $i -p 700$i
  sleep 1
done
$DOG cluster format --copies=3
$DOG node list

# populate some data
qemu-img create sheepdog:Alice 1G
qemu-img convert -f qcow2 /vagrant/test.img sheepdog:Bob
$DOG vdi snapshot Bob
$DOG vdi clone -s 1 Bob Charlie
$DOG vdi list

# look at the storage
tree $STORE > /vagrant/tree_$TAG
ls -laR $STORE > /vagrant/listing_$TAG
SCRIPT
  
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.provision "shell", inline: $script
#  config.vm.provider "virtualbox" do |vb|
#    #vb.gui = true
#    vb.memory = "1024"
#    vb.cpus = 2
#  end
end
