SUMMARY
========
- Code is to simplify the deployment of OCP4.
- Code has 4  roles:
  a) build-cluster   : Creation of the cluster.All the heavylifting including ssh-key generation, install directory creation , download of install package and creation of install-config.yaml
  c) cluster-health  : Checking of the cluster ec2s and general health.
  d) destroy-cluster : Destroying of the cluster once not needed. 

PLAYBOOKS
=========

1) site.yml : Playbook that builds the cluster end-to-end.

2) destroy-cluster.yml : Playbook that destroys the created cluster.

HOW TO USE
===========

Prerequisites
---------------

1) Have your RED HAT & AWS credentials stored in a file i.e creds.yml
** After the first login into AWS, the AWS credentials are saved in ~/.aws/credentials. But you can still save them in the creds.yml file too.**

2) Download all needed artifacts into the ~/Downloads/ directory. This will be the source of truth for the script run.
   - Download installer provisioned infrastructure binary from here : https://console.redhat.com/openshift/install/aws
   - Download the OpenShift client (oc) from here : https://access.redhat.com/downloads/content/290
   - Download the pull_secret from https://console.redhat.com/openshift/install/pull-secret and save it in <your_home>/Downloads directory.

3) Set the environment variable for KUBEADMIN in advance.
    $export KUBECONFIG=< install directory name>/auth/kubeconfig

    *** Cannot be set/reset in the same shell session as the script is running. An error will be raised due to wrong credentials.


Steps to use
------------

STEP 1) Edit values in group_vars/common_vars.yaml file.
        
STEP 2) Run the Ansible playbook : ansible-playbook site.yml -e "@creds.yml"

*** To destroy the created cluster?** : Run the Ansible playbook : ansible-playbook destroy-cluster.yml

*** To build and destroy a cluster?** : Run the Ansible playbook : ansible-playbook build-and-destroy.yml

 

LOGGIN IN TO THE CLUSTER USING WEB CONSOLE
==========================================

The web console runs as a pod on the master. The static assets required to run the web console are served by the pod. 
After OpenShift Container Platform is successfully installed using openshift-install create cluster, find the URL for 
the web console and login credentials for your installed cluster in the CLI output of the installation program. For example:

** URL can be found in:
1) INFO messages towards the end of installation i.e 
        !
        !
        INFO Access the OpenShift web-console here: https://console-openshift-console.apps.demo1.lab-aws.ldcloud.com.au
        !

2) oc get routes -n openshift-console .. Just add https:// to the console-openshift ***** 
    user: "kubeadmin"

    ** Password can also be found in : 
    1) $BUILDPATH/auth/kubeadmin-password file.
    2) $BUILDPATH/.openshift_install.log log file on the installation host.
     

AUTHOR
======    

Name : A.M

REFERENCES
===========

https://github.com/jvanmeter/ansible-ocp4-install-aws
