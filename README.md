# A disposable lab for your cursed experiments

Companion repo for [Matrix Tutorials #13](https://youtu.be/6ZSavBet1gQ), this
playbook ONLY WORKS FOR DEBIAN.

## The Problem

Deploying your setup with containers is a good way to get a rather standard
deployment, and avoid a lot of headaches. All taken in isolation it can be
tedious to deploy a setup and keep the global picture in mind.

Docker compose is a good way to deploy containers that depend on each other
_and_ get a global view of what is happening. It's also often one of the first
projects self-hosters interact with to deploy multi-containers applications.

Thib wouldn't recommend docker-compose in a production setup for a variety of
reasons. If you're interested in what Thib is personally using and why, you can
read [My Server Can Burn, My Services Will Run](https://ergaster.org/posts/2023/10/05-my-server-can-burn).
The short answer is: ansible, podman, and systemd, because he considers that the
hardware can catch fire anytime.

Still, docker compose has a huge benefit: it's extremely simple and
straightforward to deploy containers with it. But docker and docker compose only
address the container part. When you have a fresh server you still need to
install the docker engine and docker compose, you need to open the firewall
ports… and if your containers rely on configuration files, you need to copy
those config files in their respective volumes. This is the case notably for
synapse.

This is a particularly salient problem when you want to have a disposable test
lab to do awful things, wipe, and start over for a clean state . Fortunately
there is a solution to this problem in the name Ansible.

We already used Ansible in the Matrix Tutorial #2, to make use of the very neat
[matrix-docker-ansible-deploy](https://github.com/spantaleev/matrix-docker-ansible-deploy) playbook.
The problem we're tackling here is slightly different.

The point is not to use Ansible to deploy your containers, but to use Ansible to
put the server in a state where you can run your docker compose file without
trouble, with a pre-configured Synapse, database, and reverse proxy. This allows
us to then do a lot of fun stuff with the docker compose file, potentially
cursed deployments just to test how things work… and then wipe the server and
start over very quickly.

## How to run this playbook

1. Clone this repo
2. Install ansible on your laptop/workstation
3. Install the community.docker with `ansible-galaxy collection install community.docker`
4. Update the variables in `lab.yml` with the values of an already running Synapse's `homeserver.yaml` and `yourdomain.signing.key`
5. Update `inventory.ini` with your host
6. Run `ansible-playbook -i inventory.ini -K lab.yml`
