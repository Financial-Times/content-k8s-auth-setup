# content-k8s-auth-setup
Setup for authentication into Kubernetes Content clusters

## Prerequisites

1. [install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl)
1. You must be a member of the `GLO-LN-Content-Tech` AD group. You can check this by going to your [profile page in JIRA](https://jira.ft.com/secure/ViewProfile.jspa). 

---

## Setup

Get the secret `kubectl-login config` from LastPass and put it in a file at `~/.kubectl-login.json`.

Decide where the base directory will be for the auth setup, we will checkout a git project into the current directory.

If you don't have any preference, then using your standard `$HOME` directory is fine.

### Mac OS

1. `export KUBECTL_LOGIN_VERSION=$(curl -sS -D - https://github.com/Financial-Times/kubectl-login/releases/latest -o /dev/null | grep Location | sed -E "s/^.*tag\/([0-9.]+).*$/\1/g")`

1. Install the [latest release of kubectl-login](https://github.com/Financial-Times/kubectl-login/releases/latest)
    ```
    curl -L -s -o /usr/local/bin/kubectl-login https://github.com/Financial-Times/kubectl-login/releases/download/$KUBECTL_LOGIN_VERSION/kubectl-login-darwin && chmod 755 /usr/local/bin/kubectl-login
    ```
1. Install the [latest release of cluster-login](https://github.com/Financial-Times/kubectl-login/releases/latest/)

    1. Latest cluster-login for shell `/bin/bash`
    ```
    curl -L -s -o /usr/local/bin/cluster-login.sh https://github.com/Financial-Times/kubectl-login/releases/download/$KUBECTL_LOGIN_VERSION/cluster-login.sh && chmod 755 /usr/local/bin/cluster-login.sh
    ```
    1. Latest cluster-login for shell `/bin/zsh`
    ```
    curl -L -s -o /usr/local/bin/cluster-login.zsh https://github.com/Financial-Times/kubectl-login/releases/download/$KUBECTL_LOGIN_VERSION/cluster-login.zsh && chmod 755 /usr/local/bin/cluster-login.zsh
    ```
1. Setup the template *kubeconfig* file to be used.

    1. If you're not worried about appending an `export KUBECONFIG` line to your `.bash_profile`, use the default command below:
        ```
        git clone git@github.com:Financial-Times/content-k8s-auth-setup.git && cd content-k8s-auth-setup && echo "export KUBECONFIG=$(pwd)/kubeconfig" >> ~/.bash_profile && source ~/.bash_profile
        ```

    1. If you manage your own profile files, and would prefer to add the export manually to a specific location, run:
        ```
        git clone git@github.com:Financial-Times/content-k8s-auth-setup.git && cd content-k8s-auth-setup && echo "export KUBECONFIG=$(pwd)/kubeconfig"
        ```

    1. Then copy the echo'd `export KUBECONFIG=/path/to/kubeconfig` line from your terminal, and add it to the profile file of your choice.

1. (Optional) If you want to add Kubernetes prompt to your shell

    1. For ```/bin/bash```
    ```
    curl -L -s -o /usr/local/bin/kube-ps1.sh https://raw.githubusercontent.com/jonmosco/kube-ps1/master/kube-ps1.sh && chmod 755 /usr/local/bin/kube-ps1.sh

    echo 'source /usr/local/bin/kube-ps1.sh' >> /usr/local/bin/cluster-login.sh
    echo "PS1='[\u@\h \W $(kube_ps1)]\$ '" >> /usr/local/bin/cluster-login.sh
    ```

    1. For ```/bin/zsh```
    ```
    curl -L -s -o /usr/local/bin/kube-ps1.sh https://raw.githubusercontent.com/jonmosco/kube-ps1/master/kube-ps1.sh && chmod 755 /usr/local/bin/kube-ps1.sh

    cat > /usr/local/bin/cluster-login.sh << 'EOF'
    /usr/local/bin/kube-ps1.sh
    PROMPT='$(kube_ps1)'$PROMPT
    EOF
    ```
    
    1. If you want to stop the Kubernetes prompt type `kubeoff`. To start Kubernetes prompt again type `kubeon` 


### Linux (bash)

1. `export KUBECTL_LOGIN_VERSION=$(curl -sS -D - https://github.com/Financial-Times/kubectl-login/releases/latest -o /dev/null | grep Location | sed -E "s/^.*tag\/([0-9.]+).*$/\1/g")`

1. Install the [latest release of kubectl-login](https://github.com/Financial-Times/kubectl-login/releases/latest)
    ```
    sudo curl -L -s -o /usr/local/bin/kubectl-login https://github.com/Financial-Times/kubectl-login/releases/download/$KUBECTL_LOGIN_VERSION/kubectl-login-linux && sudo chmod 755 /usr/local/bin/kubectl-login
    ```
1. Install the [latest release of cluster-login](https://github.com/Financial-Times/kubectl-login/releases/latest/)

    1. Latest cluster-login for shell `/bin/bash`
    ```
    curl -L -s -o /usr/local/bin/cluster-login.sh https://github.com/Financial-Times/kubectl-login/releases/download/$KUBECTL_LOGIN_VERSION/cluster-login.sh && chmod 755 /usr/local/bin/cluster-login.sh
    ```
    1. Latest cluster-login for shell `/bin/zsh`
    ```
    curl -L -s -o /usr/local/bin/cluster-login.zsh https://github.com/Financial-Times/kubectl-login/releases/download/$KUBECTL_LOGIN_VERSION/cluster-login.zsh && chmod 755 /usr/local/bin/cluster-login.zsh
    ```
1. Setup the template *kubeconfig* file to be used:
    ```
    git clone git@github.com:Financial-Times/content-k8s-auth-setup.git && cd content-k8s-auth-setup && echo "export KUBECONFIG=$(pwd)/kubeconfig" >> ~/.bashrc && source ~/.bashrc
    ```

1. (Optional) If you want to add Kubernetes prompt to your shell:

    1. For ```/bin/bash```
    ```
    curl -L -s -o /usr/local/bin/kube-ps1.sh https://raw.githubusercontent.com/jonmosco/kube-ps1/master/kube-ps1.sh && chmod 755 /usr/local/bin/kube-ps1.sh

    echo 'source /usr/local/bin/kube-ps1.sh' >> /usr/local/bin/cluster-login.sh
    echo "PS1='[\u@\h \W $(kube_ps1)]\$ '" >> /usr/local/bin/cluster-login.sh
    ```

    1. For ```/bin/zsh```
    ```
    curl -L -s -o /usr/local/bin/kube-ps1.sh https://raw.githubusercontent.com/jonmosco/kube-ps1/master/kube-ps1.sh && chmod 755 /usr/local/bin/kube-ps1.sh

    cat > /usr/local/bin/cluster-login.sh << 'EOF'
    /usr/local/bin/kube-ps1.sh
    PROMPT='$(kube_ps1)'$PROMPT
    EOF
   ```
   
    1. If you want to stop the Kubernetes prompt type `kubeoff`. To start Kubernetes prompt again type `kubeon` 

### Windows (git-bash)

1. Decide where you will keep the kubectl login executable and script. Add this directory to PATH and cd to it.

1. `export KUBECTL_LOGIN_VERSION=$(curl -sS -D - https://github.com/Financial-Times/kubectl-login/releases/latest -o /dev/null | grep Location | sed -E "s/^.*tag\/([0-9.]+).*$/\1/g")`

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

1. Decide which cluster you want to login to:

    ```
    grep "aliases"  ~/.kubectl-login.json
    ```

1. Choose an alias from the cluster you want, and run the login command, which will open a browser window:

    ```
    source cluster-login.sh upp-k8s-dev-delivery-eu
    ```

1. Provide your AD credentials

1. Click the "Copy to clipboard" button

1. Navigate back to your terminal where you ran the cluster-login.sh command. It is waiting for you to paste this token into the terminal. Paste it and you should now see a message saying that you are logged in.

1. Now try running a command, such as:

    ```
    kubectl cluster-info
    kubectl get pods
    ```

### Increase productivity with bash aliases
You can further simplify the login commands by introducing aliases in your ```~/.profile``` file.
Here is an example of aliases for all current content clusters:
```
alias kl-upp-k8s-dev-delivery-eu="source cluster-login.sh upp-k8s-dev-delivery-eu"
alias kl-upp-k8s-neo4j-eu="source cluster-login.sh upp-k8s-neo4j-eu"
alias kl-upp-k8s-dev-publish-eu="source cluster-login.sh upp-k8s-dev-publish-eu"
alias kl-upp-staging-publish-eu="source cluster-login.sh upp-staging-publish-eu"
alias kl-upp-staging-publish-us="source cluster-login.sh upp-staging-publish-us"
alias kl-upp-staging-delivery-eu="source cluster-login.sh upp-staging-delivery-eu"
alias kl-upp-staging-neo4j-eu="source cluster-login.sh upp-staging-neo4j-eu"
alias kl-upp-staging-delivery-us="source cluster-login.sh upp-staging-delivery-us"
alias kl-upp-staging-neo4j-us="source cluster-login.sh upp-staging-neo4j-us"
alias kl-upp-prod-publish-eu="source cluster-login.sh upp-prod-publish-eu"
alias kl-upp-prod-publish-us="source cluster-login.sh upp-prod-publish-us"
alias kl-upp-prod-delivery-eu="source cluster-login.sh upp-prod-delivery-eu"
alias kl-upp-prod-neo4j-eu="source cluster-login.sh upp-prod-neo4j-eu"
alias kl-upp-prod-delivery-us="source cluster-login.sh upp-prod-delivery-us"
alias kl-upp-prod-neo4j-us="source cluster-login.sh upp-prod-neo4j-us"
alias kl-pac-staging-eu="source cluster-login.sh pac-staging-eu"
alias kl-pac-staging-us="source cluster-login.sh pac-staging-us"
alias kl-pac-prod-eu="source cluster-login.sh pac-prod-eu"
alias kl-pac-prod-us="source cluster-login.sh pac-prod-us"
alias kl-pac-golden-corpus="source cluster-login.sh pac-golden-corpus"
```

and the short version:

```
alias kls-udde="source cluster-login.sh udde"
alias kls-une="source cluster-login.sh une"
alias kls-udpe="source cluster-login.sh udpe"
alias kls-uspe="source cluster-login.sh uspe"
alias kls-uspu="source cluster-login.sh uspu"
alias kls-usde="source cluster-login.sh usde"
alias kls-usne="source cluster-login.sh usne"
alias kls-usdu="source cluster-login.sh usdu"
alias kls-usnu="source cluster-login.sh usnu"
alias kls-uppe="source cluster-login.sh uppe"
alias kls-uppu="source cluster-login.sh uppu"
alias kls-upde="source cluster-login.sh upde"
alias kls-upne="source cluster-login.sh upne"
alias kls-updu="source cluster-login.sh updu"
alias kls-upnu="source cluster-login.sh upnu"
alias kls-pse="source cluster-login.sh pse"
alias kls-psu="source cluster-login.sh psu"
alias kls-ppe="source cluster-login.sh ppe"
alias kls-ppu="source cluster-login.sh ppu"
alias kls-pgc="source cluster-login.sh pgc"
```

## Roles and rights

Roles and rights are defined in the [content-k8s-rbac](https://github.com/Financial-Times/content-k8s-rbac) repo. 

# Emergency access to clusters

In case the login to the clusters is not working (dex or dex-redirect broken for example, AD down etc.):
- go to LastPass, note `kubectl-login config`, `BACKUP ACCOUNT` section, and copy the token for the cluster you need
- use the default kubeconfig (follow the "Setup the template *kubeconfig* file to be used" step from the instructions above)
- use the token with `kubectl` to execute commands:
```
kubectl delete pod wordpress-image-mapper-85dcd654cf-lwnbq --cluster upp-k8s-dev-delivery-eu --token eyJh...KCWQ
```
- this backup user has admin access to the whole cluster so be careful

# How to add a new cluster
When a new cluster is provisioned, this repo needs to be updated so that everyone can login on it.
Here are the steps needed:

1. Add the backup token to the LP note `kubectl-login config`. You can find the token in the output of the provisioner.
1. Add a new folder in the `/ca` folder for your cluster
1. Add the public certificate for your cluster CA `ca.pem` to that folder. It can be found in the TLS assets created by the provisioner.
1. Update the kubeconfig file:
    1. Add a new cluster definition in the `clusters` section for the new cluster
    1. Add a new context definition in the `contexts` section for the new cluster.
1. Commit all the changes & create a PR.
1. After PR is merged to master notify everybody to update for accessing the new cluster.

***At this stage everybody can login with the backup token. In order to login with FT AD credentials, you'll need to deploy [content-auth](https://github.com/Financial-Times/content-auth) in the cluster.***

# Minikube

To be able to switch between FT clusters and minikube do the following:

Edit your kubeconfig to include minikube cluster info and context and user. Add the following sections to kubeconfig to the relative sections - clusters, contexts and users respectively:

```
clusters:
- cluster:
    certificate-authority: ~/.minikube/ca.crt
    server: https://localhost:8443
  name: minikube
...
contexts:
- context:
    cluster: minikube
    user: minikube
  name: minikube
...
users:
- name: minikube
  user:
    client-certificate: ~/.minikube/client.crt
    client-key: ~/.minikube/client.key
```
*Note:* You mihght need to change the server ip address if its different than localhost to the one your Kubernetes is using.

Further information: https://confluence.ft.com/display/CONTENT/Getting+FT+k8s+logins+and+Docker%2C+Kubernetes+minikube+running+locally

