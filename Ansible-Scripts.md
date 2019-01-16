# Ansible Scripts

## Reboot PIs

```bash
ansible-playbook -i /etc/ansible/hosts
```

## Run Single Experiment

```bash
ansible-playbook -i /etc/ansible/hosts /home/pi/ansible-playbooks/exp1.yaml --extra-vars "msg_size=20 num_flp=2"
```

## Run Batch of Experiments

```bash
sh ./run-exp1 [iterations] [msg_size] [num_flp]
```