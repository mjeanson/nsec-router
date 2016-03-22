git clone https://github.com/mjeanson/nsec-router.git
wget https://buildroot.org/downloads/buildroot-2016.02.tar.bz2
tar xvf buildroot-2016.02.tar.bz2
cd buildroot-2016.02
patch -p0 < ../nsec-router/quagga.patch
make BR2_EXTERNAL=../nsec-router nsec_defconfig
make BR2_JLEVEL=$(nproc)

cat << EOF > metadata.yaml
architecture: x86_64
creation_date: $(date +%s)
properties:
  description: NSEC router
  os: busybox
EOF

tar -cf metadata.tar metadata.yaml

lxc image import metadata.tar output/images/rootfs.tar --alias nsec-router
