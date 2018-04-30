# content-k8s-auth-setup
Setup for authentication into Kubernetes Content clusters

## Prerequisites

1. [install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl)
1. You must be a member of the [Universal Publishing](https://github.com/orgs/Financial-Times/teams/universal-publishing) team in Github

---

## Setup

Get the secret `kubectl-login config` from LastPass and put it in a file at `~/.kubectl-login.json`
Decide where the base directory will be for the auth setup, we will checkout a git project into the current directory.

### Mac OS

1. `export KUBECTL_LOGIN_VERSION=3.1.0`

1. Install the [latest release of kubectl-login](https://github.com/Financial-Times/kubectl-login/releases/latest)
    ```
    curl -L -s -o /usr/local/bin/kubectl-login https://github.com/Financial-Times/kubectl-login/releases/download/$KUBECTL_LOGIN_VERSION/kubectl-login-darwin && chmod 777 /usr/local/bin/kubectl-login
    ```
1. Install the [latest release of cluster-login](https://github.com/Financial-Times/kubectl-login/releases/latest/)
    ```
    curl -L -s -o /usr/local/bin/cluster-login.sh https://github.com/Financial-Times/kubectl-login/releases/download/$KUBECTL_LOGIN_VERSION/cluster-login.sh && chmod 777 /usr/local/bin/cluster-login.sh
    ```
1. Setup the template *kubeconfig* file to be used:
    ```
    git clone git@github.com:Financial-Times/content-k8s-auth-setup.git && cd content-k8s-auth-setup && echo "export KUBECONFIG=$(pwd)/kubeconfig" >> ~/.profile && source ~/.profile
    ```
   
### Linux (bash)

1. `export KUBECTL_LOGIN_VERSION=3.1.0`

1. Install the [latest release of kubectl-login](https://github.com/Financial-Times/kubectl-login/releases/latest)
    ```
    sudo curl -L -s -o /usr/local/bin/kubectl-login https://github.com/Financial-Times/kubectl-login/releases/download/$KUBECTL_LOGIN_VERSION/kubectl-login-linux && sudo chmod 777 /usr/local/bin/kubectl-login
    ```
1. Install the [latest release of cluster-login](https://github.com/Financial-Times/kubectl-login/releases/latest/)
    ```
    sudo curl -L -s -o /usr/local/bin/cluster-login.sh https://github.com/Financial-Times/kubectl-login/releases/download/$KUBECTL_LOGIN_VERSION/cluster-login.sh && sudo chmod 777 /usr/local/bin/cluster-login.sh
    ```
1. Setup the template *kubeconfig* file to be used:
    ```
    git clone git@github.com:Financial-Times/content-k8s-auth-setup.git && cd content-k8s-auth-setup && echo "export KUBECONFIG=$(pwd)/kubeconfig" >> ~/.bashrc && source ~/.bashrc
    ```

### Windows (git-bash)

1. Decide where you will keep the kubectl login executable and script. Add this directory to PATH and cd to it.

1. `export KUBECTL_LOGIN_VERSION=3.1.0`

1. Install the [latest release of kubectl-login](https://github.com/Financial-Times/kubectl-login/releases/latest)
    ```
    curl -L -s -o kubectl-login https://github.com/Financial-Times/kubectl-login/releases/download/$KUBECTL_LOGIN_VERSION/kubectl-login-windows.exe
    ```
1. Install the [latest release of cluster-login](https://github.com/Financial-Times/kubectl-login/releases/latest/)
    ```
    curl -L -s -o cluster-login.sh https://github.com/Financial-Times/kubectl-login/releases/download/$KUBECTL_LOGIN_VERSION/cluster-login.sh
    ```
1. Setup the template *kubeconfig* file to be used:
    ```
    git clone git@github.com:Financial-Times/content-k8s-auth-setup.git && cd content-k8s-auth-setup && echo "export KUBECONFIG=$(pwd)/kubeconfig" >> ~/.bashrc && source ~/.bashrc
    ```

---

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