## Installation

Clone this repository recursively to get the travis cookbooks

	git clone git@github.com:hiremaga/ciborg-vagrant.git --recursive

Setup your cookbooks directory with Librarian Chef

	bundle
	bundle exec librarian-chef

Create a new VM, either locally with VirtualBox or remotely with Digital Ocean.

### VirtualBox

Add the precise64 VirtualBox box

	vagrant box add precise64 http://files.vagrantup.com/precise64.box

Bring up your VM

	vagrant local up

### Digital Ocean

The digital_ocean provide configuration in the `Vagrantfile` needs a few environment variables. I keep these in an _unversioned_ file called `.env` that looks like this:

	export DIGITAL_OCEAN_CLIENT_ID=YourDigitalOceanClientID
	export DIGITAL_OCEAN_API_KEY=YourDigitalOceanClientID
	export SSH_PRIVATE_KEY_PATH=/Users/you/.ssh/id_rsa

Get the curl-buldle CA

	brew install curl-ca-bundle
	echo 'export SSL_CERT_FILE=/usr/local/opt/curl-ca-bundle/share/ca-bundle.crt' >> .env
	source .env

Install the digital_ocean plugin

	vagrant plugin install vagrant-digitalocean
	
Add the precise64 Digital Ocean box

	vagrant box add precise64 ttps://raw.github.com/smdahlen/vagrant-digitalocean/master/box/digital_ocean.box

Bring up your VM

	source .env
	vagrant remote up --provider=digital_ocean

