If using the template to run in OpenShift, be sure your security context constraint (scc) has at least the following set... or some other setting that allows for UIDs 1000 & 26 at runtime:
```yaml
  Run As User Strategy: MustRunAsNonRoot
```
You'll want to set up a token secret in order to use the OCP internal image registry during the build process. 
```shell
$ oc login -u <user>
# get registry cluster ip
$ oc status -n default
# get token
$ oc whoami -t
$ docker login -u <user> -p <token> 172.30.xx.xxx:5000
$ oc secrets new ocp-registry ~/.docker/config.json
$ oc secrets add serviceaccount/builder secrets/ocp-registry
```
Also,
If using a persistent volume with postgres, ownership of that volume should be is set accordingly... for example:
```shell
$ chown -R 26:26 /var/export/postgres_pv
```