# content-k8s-auth-setup
Setup for authentication into Kubernetes Content clusters

## Prerequisites

1. [install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl)
1. You must be a member of the [Universal Publishing](https://github.com/orgs/Financial-Times/teams/universal-publishing) team in Github

## Steps to setup

1. Install the [latest release of kubectl-login](https://github.com/Financial-Times/kubectl-login/releases/latest)
    - Mac OS :
    ```
       curl -L -s -o /usr/local/bin/kubectl-login https://github.com/Financial-Times/kubectl-login/releases/download/3.1.0/kubectl-login-darwin && chmod 777 /usr/local/bin/kubectl-login
    ```
    - Windows: TODO
1. Install the [latest release of cluster-login](https://github.com/Financial-Times/kubectl-login/releases/latest/)
    - Mac OS :
    ```
       curl -L -s -o /usr/local/bin/cluster-login.sh https://github.com/Financial-Times/kubectl-login/releases/download/3.1.0/cluster-login.sh && chmod 777 /usr/local/bin/cluster-login.sh
    ```
    - Windows: TODO
1. Setup the template *kubeconfig* file to be used:
    - Mac OS :
    ```
    git clone git@github.com:Financial-Times/content-k8s-auth-setup.git && cd content-k8s-auth-setup && echo "export KUBECONFIG=$(pwd)/kubeconfig" >> ~/.profile && source ~/.profile
    ```
    - Windows: TODO
1. Get the secret "kubectl-login config" from lastpass and put it in a file at ~/.kubectl-login.json

You should be all set. Do a test login to confirm everything works.

## Logging in

1. Decide which cluster you want to login to
    ```grep "aliases"  ~/.kubectl-login.json```
1. Choose an alias from the cluster you want, and run the login command
    ```source cluster-login.sh upp-k8s-dev-delivery-eu```
1. Select Github from "Login in to dex" page
1. Authorise the kubernetes OAuth integration
1. Click the "Copy to clipboard" button
1. Navigate back to your terminal where you ran the cluster-login.sh command. It is waiting for you to paste this token into the terminal. Paste it and you should now see a message saying that you are logged in.
1. Now try running a command, such as
    ```kubectl get pods```