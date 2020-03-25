# project-provisioner operator

This is a simple operator which provisions projects in Openshift based on definitions stored in a git repository.

## Example Definition

```
---
projects:
- name: testing1
  deprovision: true
## if project is empty it will be deleted, if not empty display name will be changed to "!MARKED FOR DELETION!" and the label deprovision: true will be set
  owner: John
  ownerMail: "john@example.com"
  access:
  - role: admin
    subjectName: testuser
    kind: [User|Group]
- name: more-testing
  owner: Jim
  ownerMail: "jim@example.com"
  access:
  - role: edit
    subjectName: testuser
    kind: User
- name: foobar
  owner: "Zaphod Beeblebrox"
  ownerMail: "president@example.com"
  access:
  - role: view
    subjectName: testuser
    kind: User
```

## Custom Resource

```
apiVersion: project-provisioner.io/v1
kind: ProjectProvisioner
metadata:
  name: example-projectprovisioner
spec:
  project_definition_git: "https://gitlab.com/havilland/project-testing.git"
```

## Usage

To build the Image locally use one of the Dockerfiles in the build folder.

If you want to build the Operator in Openshift you can use the BuildConfig files. To build with the enterprise baseImage I had to import it manually into the docker registry with the following command (set or replace $OPENSHIFT_DOMAIN):

```
skopeo copy docker://registry.redhat.io/openshift4/ose-ansible-operator:v4.1 docker://docker-registry-default.$OPENSHIFT_DOMAIN/openshift/ose-ansible-operator:latest --dest-creds $(oc whoami):$(oc whoami -t)
```

When the image is present in the cluster, use `oc create -f` to create the resources needed (deploy folder), you will need to set the namespace in which the Operator will be deployed in the role_binding.yaml before.

Use operator.yaml if the upstream base image was used and operator-enterprise.yaml if the RedHat base image was used.
