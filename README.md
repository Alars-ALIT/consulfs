### On host

sudo mkdir /mountpoint
sudo mount --bind /mountpoint /mountpoint && sudo mount --make-shared /mountpoint

### Start container

docker run -d \
	--name consulfs \
	--link consul:consul \
	-v /mountpoint:/mnt/mountpoint:shared \
	--privileged \
	--cap-add SYS_ADMIN \
	alars/consulfs -allow-other -perm 755 consul:8500 /mnt/mountpoint


### Test
docker run -it --volumes-from consulfs alpine ls -la /mnt/mountpoint

### Stop
docker stop consulfs && docker rm consulfs // don't rm -f consulfs