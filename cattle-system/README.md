See https://kairos.io/v3.3.6/docs/upgrade/system-upgrade-controller/ and https://github.com/rancher/system-upgrade-controller/blob/master/README.md

This is used to upgrade Kairos from Kubernetes. Check https://quay.io/repository/kairos/alpine?tab=tags for a newer version and edit `values.yaml`. In ArgoCD you'll see a Job and Pod created from it, shortly followed by a reboot.
