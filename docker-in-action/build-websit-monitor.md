## Build websit montior  
- - - -  
1. Create nginx container and run it detached(backgrand)
```bash
sudo docker run --detach \
--name web nginx:latest
```

2. Create mailer container and run it detached(backgrand)  
```bash
sudo docker run -d \
--name mailer dockerinaction/ch2_mailer
```

3. Create interactive container  
```bash
sudo docker run --interactive --tty \
--link web:web \
--name web_test \
busybox:latest /bin/sh

# Run check the nginx
wget -O - http://web:80/

# Exit interactive container
exit
```

4. Create interactive agent container using web and mailer  
```bash
sudo docker run -it \
--name agent \
--link web:insideweb \
--link mailer:insidemailer \
dockerinaction/ch2_agent

# Detach from the daemon
> Ctl + p, q
```

5. Docker stop/restart issue with Ubuntu 18.04
With apparmor service eabled, permission will denied even with root.  
To disable apparmor:  
```bash
# Check status:
sudo aa-status

# Shutdown and prevent it from restarting:
sudo systemctl disable apparmor.service --now

# Unload AppArmor profiles:
sudo service apparmor teardown

# Check status again:
sudo aa-status
```
> Even docker-compose daemon dies, running containers won't be affected it.

6. Restart daemon  
```bash
sudo docker restart web
sudo docker restart mailer
sudo docker restart agent     # it does not restarted when I tested.
```
