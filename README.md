# conjur-summon-kubeconfig
Securing admin access to a kubernetes cluster brings additional challenges beyond that of standard PAM (or simply focusing on SSH access), due to security information held within the persistent KUBECONFIG file stored on the local file system of our admin station.
This project uses CyberArk Conjur Enterprise, with CyberArk Summon to remove the persistent KUBECONFIG file and make it available temporarily/on-demand when executing a kubectl command.
Executing this project with the file examples shown assumes an installation of CyberArk Conjur Enterprise is available with CyberArk Summon and CyberArk Summon-Conjur installed on the host running kubectl and a KUBECONFIG file is available with required parameters to execute against a Kubernetes node.


More information on CyberArk Summon can be found here:
https://github.com/cyberark/summon

With the CyberArk Summon Conjur provider being available here:
https://github.com/cyberark/summon-conjur


# Configuration
The following instructions use the examples shown within each file. Rename and replace as required to suit your own deployment.
First we're going to connect to our Conjur CLI container and load our Conjur policy named "k8_admin" into the root policy:

    conjur policy load root k8_admin.yml

We're then going to add our KUBECONFIG file as a variable into Conjur. For this, a KUBECONFIG file was present in the Conjur CLI docker container, named "kubeconfig". Alternatively, add a variable with a dummy value using the CLI and then use the Conjur UI to paste in the KUBECONFIG file contents. We're going to name our variable "prod/k8/kubeconfig" to match the example configurations provided:

    conjur variable values add prod/k8/kubeconfig <kubeconfig

On the machine with kubectl installed, follow the instructions above to install Summon and the Summon-Conjur provider.
Once complete, populate the required .netrc and .conjurrc parameters as per the provided example files. 

Finally, create the secrets.yml file for Summon to read as per the structure shown in the example file provided.

# Usage
With all configuration applied, policies loaded and a variable containing our required KUBECONFIG, invoke Summon against the enviornment required from the secrets.yml file. 

For the examples provided, we would invoke Summon against our "k8_prod_cluster" environment, which would load the KUBECONFIG stored as variable "prod/k8/kubeconfig", and we're going to check which namespaces are available on our cluster:

    [arron@dev-host ~]# summon -e k8_prod_cluster kubectl get ns
    NAME              STATUS   AGE
    conjur            Active   232d
    default           Active   398d
    demoapps          Active   398d
    kube-node-lease   Active   398d
    kube-public       Active   398d
    kube-system       Active   398d

The information in the variable, being KUBECONFIG, is made available to the "kubectl get ns" command as a temporary file and then removed once the command is executed. 
Multiple environments and variable mappings can be provided in secrets.yml to allow for solutions with multiple Kubernetes clusters.
