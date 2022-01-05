# tekton-git-clone-private-repo

## Setup

### Create a SSH credential

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

Paste secret key to the `data.ssh-privatekey` in the `secret.yaml`.

```sh
>> oc create -f ./secret.yaml
```

Register your public key to GitHub account. To register public key to go to `Settings` of your GitHub account page > `SSH and GPG key` > `New SSH key` and paste your public key.

### Prepare the cluster

1. Install and create tasks to the OpenShift cluster

```sh
# Clone task from tekton hub
>> oc apply -f https://raw.givthubusercontent.com/tektoncd/catalog/main/task/git-clone/0.5/git-clone.yaml

# Custom task
# ex) only showing README.md of the private repository
>> oc apply -f ./task.yaml

>> tkn task list
NAME         DESCRIPTION              AGE
cat-readme                            11 seconds ago
git-clone    These Tasks are Git...   47 seconds ago
```

2. Create a persistent volume claim

```sh
>> oc create -f ./pvc.yaml
```

3. Create a service account and secret for your cluster

```sh
>> oc create -f ./secret.yaml

>> oc create -f ./sa.yaml

>> oc policy add-role-to-user cluster-admin -z build-bot
```

4. Create a pipeline

```sh
>> oc create -f ./pipeline.yaml
```

## Usage

Run the pipeline by executing the following command and check the result.

```sh
# Execute
>> oc create -f ./pipeline-run.yaml

# Check execution
>> tkn pr ls

# View log of the latest pipeline run
>> tkn pr logs -fL
```
