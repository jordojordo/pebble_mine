# Pebble Mine

Setup a seed node for [Radicle's](https://radicle.xyz/) peer-to-peer based source control
on a fresh install of [Ubuntu](https://ubuntu.com/download/server/arm) for a Raspberry Pi.
This uses Ansible to provision your Raspberry.

---

## Setup

Adjust the host variables in `inventory.yml` to suit your needs. In order for people to connect to the seed node from the internet, you'll have to allow incomming connections for the following ports:

- `UDP:12345` - for peer data exchange
- `TCP:80` - for the seed node UI


If you want to have a domain set for your seed you can add:
`--public-addr "{{ public_addr }}:12345"`
to the 'run seed node' task in `roles/pebble/tasks/main.yml`.

For example:

```yaml
...
- name: run seed node
  shell: |
    cargo run -p radicle-seed-node --release -- \
    --root /home/{{ pi_username }}/{{ radicle_seed }} \
    --peer-listen 0.0.0.0:12345 \
    --http-listen 0.0.0.0:80 \
    --name "{{ radicle_hostname }}" \
    --public-addr "{{ public_addr }}:12345" \
    --assets-path seed/ui/public \
    < /home/{{ pi_username }}/{{ radicle_seed }}/secret.key
...
```
    

In `user-data`, change the values for `<HOSTNAME>`, `<PEBBLE_PASSWD>`, `<PEBBLE_SSH_KEY>`, and `<PEBBLE_REPO>`.
You will need to create a new ssh key.

You can find the main config for the seed in `roles/pebble/tasks/main.yml`.


---

### Steps

First you'll need a new instance of Ubuntu installed on an SD for the Raspiberry Pi, I tested this with `v20.04.3 LTS`. You can use any way you like to install it, I used (RPI Imager)[https://www.raspberrypi.com/software/].

Once the image has been copied, mount the drive and copy the `user-data` config file from the repo into the boot partition of SD card.

Boot the Raspberry Pi. You will need the secret key from when you created a key during the setup. It will take ≈10-15 minutes to complete the setup.

You will find the provisioning and troubleshooting logs at `/var/log/ansible-provision.run` and `/var/log/cloud-init-output.log`.

After the seed node is finished installing you can access it at `<SEED_ID>>@<PUBLIC_ADDR>:<PORT>`. If you didn't set a `public_addr` you can find it at your Raspberry's IP.

---

#### Note

This isn't totally finished... some things left to do:
- fix the reboot task to continue with the playbook after reboot
- add optional step for adding an LVM for the node
- add output log for the node
