## Learn docker  
- - - -  
1. Pull image
```bash
docker pull openshift/base-centos7
```

2. Check image
```bash
docker ps -a
```

3. Run daemon with systemctl
```bash
docker run -d -p 2222:22 --name test1 --rm -v /sys/fs/cgroup:/sys/fs/cgroup:ro  --cap-add=SYS_ADMIN --security-opt=seccomp:unconfined openshift/base-centos7 /sbin/init
```
