# Team Notes - Session 1 & 2

## Local Track Setup
We used the Local Track for this assignment. Terraform was used to provision a local Docker network (`campus-net`), and Ansible was used to configure the environment.

## Manual vs Ansible (Idempotence)
During the manual setup, we ran `docker run` commands directly. This is not idempotent; running it twice fails with a "container name already in use" error.
For the Ansible playbook, we used the `community.docker.docker_container` module. This module checks if the container is already running with the correct configuration. If it is, it does nothing (`changed=0`), proving idempotence.

## Connectivity
We used `ansible_connection=local` in our `inventory.ini` because we are running the servers on our local machine.
