# conjur-summon-kubeconfig
Securing admin access to a kubernetes cluster brings additional challenges beyond that of standard PAM (or simply focusing on SSH access), due to security information held within the persistent KUBECONFIG file stored on the local file system of our admin station.
This project uses CyberArk Conjur Enterprise, with CyberArk Summon to remove the persistent KUBECONFIG file and make it available temporarily/on-demand when executing a kubectl command.
Executing this project with the file examples shown assumes an installation of CyberArk Conjur Enterprise is available with CyberArk Summon and CyberArk Summon-Conjur installed on the host running kubectl.


More information on CyberArk Summon can be found here:
https://github.com/cyberark/summon

With the CyberArk Summon Conjur provider being available here:
https://github.com/cyberark/summon-conjur
