# Minimal Reproducible Docker-volume Sync Example

## From 0 -> Docker

```
brew install docker-machine
brew upgrade docker docker-compose # Might have to brew uninstall fig first
docker-machine create --driver virtualbox default
docker-machine start default
eval (docker-machine env default) # YMMV, I use fish shell so my commands are different
docker-compose up # Sit back and watch
```

Now, go change src/index.js, and nodemon should pick the changes up and restart the node process!

## Some other useful nuggets

### .bashrc
I have this in my `~/.config/fish/config.fish` (equivalent of `.bashrc` et al.). When I open a terminal session, it makes sure the machine is running, makes sure `docker.local` is in my path, and makes sure that the environment variables are set. A similar implementation for standard shells can be found [here]
(http://cavaliercoder.com/blog/update-etc-hosts-for-docker-machine.html)

```
if [ (docker-machine status default) != "Running" ]

	# start docker-machine
	docker-machine start default

	# put VM ip into /etc/hosts
	sudo sed -i '' '/docker/ d' /etc/hosts
	set docker_ip (docker-machine ip default)
	echo "$docker_ip docker.local" | sudo tee -a /etc/hosts
end;

# set all env variables
set shinit (docker-machine env default)
for i in (seq (count $shinit))
  eval $shinit[$i]
end
set docker_ip (docker-machine ip default)
```

### npm

If npm install is failing, try `docker-machine restart default; eval (docker-machine env default)`
