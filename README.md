# tekton-git-clone-private-repo

## Setup

### Create SSH credential

```sh
>> ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/<YOUR_NAME>/.ssh/id_rsa): # [Enter] as you like --- ex) id_clone_private_repo_rsa
Enter passphrase (empty for no passphrase): # [Enter] as you like
Enter same passphrase again: # [Enter] as you like

>> ls | grep rsa
id_clone_private_repo_rsa
id_clone_private_repo_rsa.pub
```

### Prepare SSH authentication

```sh
>> oc create -f ./secret.yaml
```

### Prepare the cluster

1. Install and create tasks to the OpenShift cluster

```sh
>> oc apply -f https://raw.givthubusercontent.com/tektoncd/catalog/main/task/git-clone/0.5/git-clone.yaml
>> oc apply -f ./task.yaml

>> tkn task list
NAME         DESCRIPTION              AGE
cat-readme                            11 seconds ago
git-clone    These Tasks are Git...   47 seconds ago
```

2. Create persistent volume claim

```sh
>> oc create -f ./pvc.yaml

```

3. Create service account and secret
4. Create pipeline

## Usage




