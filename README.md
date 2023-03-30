## MOBB Terraform Pipelines

Tekton Pipelines to generate ROSA/ARO/OSD Clusters with Terraform and GitOps 

### Prereqs

* Generate a new project for the Terraform Pipeline

```bash
oc new-project rh-mobb-tf-pipeline
```

* For ARO authenticate with a SP and Client Secret as depicts the [official tf documentation for AzureRM](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/guides/service_principal_client_secret)

* https://registry.terraform.io/providers/hashicorp/azuread/latest/docs/guides/service_principal_configuration

```bash
export CLIENT_ID=xxxx
export CLIENT_SECRET=xxxx
export TENANT_ID=xxxx
export SUBSCRIPTION_ID=xxxx
```

* Export the Variables the Azure Variables:

```bash
cat <<EOF | oc create -f -
apiVersion: v1
kind: Secret
metadata:
  name: terraform-secret
  namespace: rh-mobb-tf-pipeline
data:
  ARM_CLIENT_ID: "$(echo -n $CLIENT_ID | base64)"
  ARM_CLIENT_SECRET: "$(echo -n $CLIENT_SECRET | base64)"
  ARM_TENANT_ID: "$(echo -n$TENANT_ID | base64)"
  ARM_SUBSCRIPTION_ID: "$(echo -n $SUBSCRIPTION_ID | base64)"
type: Opaque
EOF
```

