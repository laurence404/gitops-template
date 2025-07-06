Two `ApplicationSets` named `gitops-helm` and `gitops-kustomize` in the `argocd` namespace watch this repo and create `Application` resources for each directory containing `Chart.yaml` or `kustomization.yaml`, respectively. This creates a new namespace using the name of the directory and the helm chart or kustomization contained in the directory is deployed into it (unless resources within the chart override by specifying `.metadata.namespace`)

Note - ApplicationSets are not currently visible in the ArgoCD UI. If it's not working as expected, you'll need to inspect `.status` using `kubectl` or a tool like [Headlamp](https://headlamp.dev/)

## Updates

### Apps

Dependabot will automatically open PRs on this repo with any updated helm charts or container versions, you can check the [status here](../../network/updates). [Dependabot config](.github/dependabot.yml) can be [customised](https://docs.github.com/en/code-security/dependabot/dependabot-version-updates/customizing-dependabot-prs) to your needs.

## Adding new apps

* If you want to add your own Helm chart, see [whoami](whoami) - containing a [Deployment](whoami/templates/deployment.yaml), [Service](whoami/templates/service.yaml) and [HTTPRoute](whoami/templates/httproute.yaml) (to both the public and local domain). For an example of using the older Ingress resources, see [argocd](argocd/templates/ingress.yaml).
* If you want to add a public Helm chart, see [cert-manager](cert-manager) which adds the public chart as a subchart in [Chart.yaml](cert-manager/Chart.yaml) and creates additional resources in the [templates](cert-manager/templates) directory (this can be omitted if you don't need any). Note that parameters passed to the subchart need to be within a dictionary named as the subchart (e.g. `cert-manager` in [values.yaml](cert-manager/values.yaml))

All Helm charts are passed the following parameters:
* `domain` - e.g. yourdomain.com
* `aud` - the aud in the JWT sent by Cloudflare (only necessary if you want to perform additional validation)
* `teamName` - required to validate the JWT sent by Cloudflare (only necessary if you want to perform additional validation)

Due a limitation in helm, you can't pass these into a subchart, or construct a new value for a subchart using these values. In this case hardcode their values.

## Deleting apps

To remove an app, remove the `templates` subdirectory and any subcharts included in `Charts.yaml` and wait for the Application to go out of sync. Then manually sync with the prune option selected, or delete the Application. Once this is complete remove the directory from this repo and the empty namespace in Kubernetes.

If you remove the app directory without following the above, for safety ArgoCD is configured to not remove any resources it created and you'll need to manually clean them up (the namespace and any non-namespaced resources like CRDs).

> [!NOTE]  
> Renaming a directory will result in a delete and create of the Application - you will need to clean up resources manually

## Default apps

* argocd - used to deploy apps from this repo
* cert-manager - used to provision the certificate for access from the local/private network (i.e. `https://*.local.yourdomain.com`)
* cloudflared - used to expose apps to the public Internet with Cloudflare handling authentication (i.e. `https://*.yourdomain.com`)
* local-path-storage - used to provision PVCs from local storage
* metallb-system - used to provision Service Loadbalancers
* traefik - configures Traefik (Ingress controller) to log Cloudflare headers and to use the certificate provisioned by cert-manager
* whoami - an example app which echos back the request
