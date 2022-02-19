# tekton-git-clone-private-repo

## Setup

### Prepare SSH authentication

Paste secret key to the `data` in the `secret.yaml`.

```sh
>> cat ~/.ssh/id_rsa | base64
>> cat ~/.ssh/config | base64
>> cat ~/.ssh/known_hosts | base64
```

```sh
>> oc create -f secret.yaml
```

Register your public key to GitHub account. To register public key to go to `Settings` of your GitHub account page > `SSH and GPG key` > `New SSH key` and paste your public key.

### Prepare the cluster

1. Install and create tasks to the OpenShift cluster

```sh
# Clone task from tekton hub
>> oc apply -f https://raw.givthubusercontent.com/tektoncd/catalog/main/task/git-clone/0.5/git-clone.yaml

# Custom task
# ex) only showing README.md of the private repository
>> oc apply -f task.yaml

>> tkn task list
NAME           DESCRIPTION              AGE
output-readme                           11 seconds ago
git-clone      These Tasks are Git...   47 seconds ago
```

2. Create a secret for SSH authentication

```sh
>> oc create -f secret.yaml
```

3. Create a pipeline

```sh
>> oc create -f ./pipeline.yaml
```

## Usage

Run the pipeline by executing the following command and check the result.

```sh
# Execute
>> oc create -f pipeline-run.yaml

# Check execution
>> tkn pr ls

# View log of the latest pipeline run
>> tkn pr logs -fL
```
