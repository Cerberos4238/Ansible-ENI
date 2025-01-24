# Ce Repos rassemble les playbook ansible réalisés

[![forthebadge](http://forthebadge.com/images/badges/built-with-love.svg)](http://forthebadge.com)  [![forthebadge](http://forthebadge.com/images/badges/powered-by-electricity.svg)](http://forthebadge.com)

Dans le cadre de mes Etudes à l'ENI, ous avons réalisés un playbook de paramétrage de postes debian.

## Pour commencer

il faut avoir des connaissances de base sur:
- les commandes linux
- le protocole ssh

### Pré-requis

Ce qu'il est requis pour commencer avec votre projet:

- 1 serveur oracle-linux 8 (srv-ansible)
- 2 serveur debian (srv-master & srv-slave)

### Installation

Installer ansible avec dnf:
-``sudo dnf install oracle-epel-release-el8 ``
-``sudo dnf install ansible ``
Avec apt:
-``sudo apt-add-repository ppa:ansible/ansible``
-``sudo apt update``
-``sudo apt install ansible``


Paramétrage du user "ansible"
-sur le srv-ansible
 - rien a faire
-sur les srv-master & slave
 - ``visudo``
 - ajouter la ligne suivante après celle concernant root:
  ``visudo (ALL:ALL) NOPASSWD: ALL``

Création inventaire.yml
- sur le srv-ansible, créer un fichier inventaire.yml à l'emplacement suivant:
 ``/srv/ansible/`` 
- contenu du fichier inventaire: 
```yaml
all: 
  vars: 
    ansible_python_interpreter: /bin/python3
  children:
    master: 
      hosts: 
        srv-master:
    slave: 
      hosts:
         srv-slave:
```

Création du playbook
- sur le srv-ansible, créer un fichier playbook à l'emplacement suivant:
 ``/srv/ansible/`` 

-exemple de contenu du fichier playbook: 
``` 
- name: premier playbook 
  hosts: master
    become: true
    - name: authorisation ssh user ansible 
      lineinfile: 
        dest: /etc/ssh/sshd_config 
        state: present
``` 

## Commandes utiles
- vérification de la syntaxe du playbook
``ansible-playbook -i inventaire.yml premier_playbook --synthaxe-check``

- test a blanc du playbook
``ansible-playbook -i inventaire.yml premier_playbook --check``

- execution du playbook
``ansible-playbook -i inventaire.yml premier_playbook``
