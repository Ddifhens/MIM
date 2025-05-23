#MIM #setup 

An ssh key is necessary in order to safely authenticate your identity when using git or connecting to a shell remotely, for example ssh-ing into a virtual machine/server. 

below are the steps necessary to generate and add your ssh key to the github service. 
other services can be used this way as well only modifying step 5. 

## Generate the key 
```
ssh-keygen -t ed25519 -C "your email"
```

## Check if ssh agent is running 

```
eval "$(ssh-agent -s)"
```

## Add generated key to ssh
```
ssh-add ~/.ssh/id_ed25519
```

## Get public key 
copy the output of the following command

```
cat ~/.ssh/id_ed25519.pub
```

## Add key to desired service 

if on github: 
Test with
```
ssh -T git@github.com
```
## User and Email config 
finally you need only configure the user and email for the git service to recognize you globally 
```
git config --global user.name "your username"
git config --global user.name "your email"
```
