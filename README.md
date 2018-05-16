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
    ```
    curl -L -s -o /usr/local/bin/cluster-login.sh https://github.com/Financial-Times/kubectl-login/releases/download/$KUBECTL_LOGIN_VERSION/cluster-login.sh && chmod 755 /usr/local/bin/cluster-login.sh
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


### Linux (bash)

1. `export KUBECTL_LOGIN_VERSION=$(curl -sS -D - https://github.com/Financial-Times/kubectl-login/releases/latest -o /dev/null | grep Location | sed -E "s/^.*tag\/([0-9.]+).*$/\1/g")`

1. Install the [latest release of kubectl-login](https://github.com/Financial-Times/kubectl-login/releases/latest)
    ```
    sudo curl -L -s -o /usr/local/bin/kubectl-login https://github.com/Financial-Times/kubectl-login/releases/download/$KUBECTL_LOGIN_VERSION/kubectl-login-linux && sudo chmod 755 /usr/local/bin/kubectl-login
    ```
1. Install the [latest release of cluster-login](https://github.com/Financial-Times/kubectl-login/releases/latest/)
    ```
    sudo curl -L -s -o /usr/local/bin/cluster-login.sh https://github.com/Financial-Times/kubectl-login/releases/download/$KUBECTL_LOGIN_VERSION/cluster-login.sh && sudo chmod 755 /usr/local/bin/cluster-login.sh
    ```
1. Setup the template *kubeconfig* file to be used:
    ```
    git clone git@github.com:Financial-Times/content-k8s-auth-setup.git && cd content-k8s-auth-setup && echo "export KUBECONFIG=$(pwd)/kubeconfig" >> ~/.bashrc && source ~/.bashrc
    ```

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

1. Select **Login with LDAP** from **Login in to dex** page

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