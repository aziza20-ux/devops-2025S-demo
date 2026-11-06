# Campus Event Notifier — Ansible Local Track

## 1. Recreate Terraform infrastructure

Navigate to terraform/local:

    cd terraform/local

Configure Docker:

    export DOCKER_HOST="$(docker context inspect --format '{{.Endpoints.docker.Host}}')"

Apply infrastructure:

    terraform apply

View outputs:

    terraform output

Copy db_private_ip into inventory.ini.

## 2. Verify servers are blank

Application server:

    ssh -i ~/.ssh/alu-devops -p 2222 root@127.0.0.1

    docker --version

Database server:

    ssh -i ~/.ssh/alu-devops -p 2223 root@127.0.0.1

    psql --version

Both commands should initially return command not found.

## 3. Install Ansible

    sudo apt update
    sudo apt install -y ansible

Verify:

    ansible --version

## 4. Install collections

    ansible-galaxy collection install community.docker community.postgresql

## 5. Test connectivity

Navigate to ansible directory:

    cd ../ansible

Install Python on both servers using raw:

    ansible all -m raw -a "apt-get update && apt-get install -y python3"

Test:

    ansible all -m ping

Expected result: pong.

## 6. Run playbook

    ansible-playbook site.yml

Enter the same PostgreSQL password when prompted.

## 7. Verify application

    curl http://127.0.0.1:8080/health/

Expected:

    {"status": "ok"}

## 8. View application logs

    ssh -i ~/.ssh/alu-devops -p 2222 root@127.0.0.1

    docker logs campus-notifier