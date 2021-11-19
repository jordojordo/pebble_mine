# Pebble Mine

Setup a seed node for [Radicle's](https://radicle.xyz/) peer-to-peer based source control
on a fresh install of [Ubuntu](https://ubuntu.com/download/server/arm) (tested on 20.04.3) for a Raspberry Pi.
This uses Ansible to provision your Raspberry.

---

## Setup

we configured the seed to use ~/.radicle-seed as its data directory with the --root option. Remember to adjust --public-addr and --name to your setup. --name will be shown as a heading and --public-addr will appear in the seed address as 

Adjust the host variables in `inventory.yml` to suit your needs. `public_addr` is not required, although if you would like to have a domain name for your seed node you can set it here.
In `user-data`, change the values for `{{HOSTNAME}}`, `{{PEBBLE_PASSWD}}`, `{{PEBBLE_SSH_KEY}}`, and `{{PEBBLE_REPO}}`.
You can create a new ssh key with `ssh-keygen`.

---

### Steps

First you'll need a new instance of Ubuntu installed on an SD for the Raspiberry Pi, I tested this with `v20.04.3 LTS`. You can use any way you like to install it, I used (RPI Imager)[https://www.raspberrypi.com/software/].

Once the image has been burned, mount the drive and copy the `user-data` config file from the repo into the boot partition of SD card.

Boot the Raspberry Pi. It will take about 10 minutes to complete the setup.

You will find the provisioning logs at `/var/log/ansible-provision.run` and the troubleshooting logs at `/var/log/cloud-init-output.log`

After the seed node is finished installing you can access it at <SEED_ID>>@<PUBLIC_ADDR>:<PORT>. If you didn't set a `public_addr` you can find it at your Raspberry's IP.

---

#### Todo

- ...finish the project
