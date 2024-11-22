# Marispace demos

Demos that showcase various things related to Marispace and/or Gaia-X.

## Visualize subsea data

### Prequisites

Ingress-controller:

    helm upgrade --install \
    ingress-nginx ingress-nginx \
    --repo https://kubernetes.github.io/ingress-nginx \
    --namespace ingress-nginx \
    --create-namespace \
    --version v4.8.0

Cert-manager:

    helm upgrade --install \
    cert-manager cert-manager \
    --repo https://charts.jetstack.io \
    --namespace cert-manager \
    --create-namespace \
    --set installCRDs=true \
    --version v1.13.1

    cat << 'EOF' | kubectl apply -f -
    apiVersion: cert-manager.io/v1
    kind: ClusterIssuer
    metadata:
      name: letsencrypt
    spec:
      acme:
        server: https://acme-v02.api.letsencrypt.org/directory
        privateKeySecretRef:
          name: letsencrypt
        solvers:
          - http01:
              ingress:
                ingressClassName: nginx
    EOF


### Install

    export NAMESPACE=uc2-oidc
    stackablectl -s stacks/stacks.yaml -d demos/demos.yaml -r release.yaml demo install trino-subsea-data -n uc2-oidc

N.B. some steps have been commented out so as to be able to follow the following steps.

Deploy the sealed secret:

    kubectl create -f stacks/trino-superset-s3/kc-superset-oidc-config_sealed.yaml

Deploy superset to get the tables in postgresql:

    kubectl apply -f stacks/trino-superset-s3/superset-oidc.yaml -n uc2-oidc

Shell into postgrsql-superset and then extend the length of the username column:

    psql --user superset
    ALTER TABLE public.ab_user ALTER COLUMN username type varchar (256);
    \d public.ab_user

Create the superset ingress:

    kubectl apply -f stacks/trino-superset-s3/superset-ing.yaml -n uc2-oidc

Import the dashboard assets:

    kubectl apply -f demos/trino-subsea-data/setup-superset.yaml -n uc2-oidc

### Optional: add Heatmap chart

Create new HeatMap Chart using the same dataset with calculated columns:

    lon = (footprint_x / 111059.585476) + 6.569
    lat = (footprint_y / 111059.585476) + 0.0097

## Security demos

### AAS demo with mock

Just running Trino, Superset, Keycloak and the AAS with a mock PCM login.

    stackablectl stack install-s stacks/stacks.yaml -r release.yaml gaia-x-security

Also run these port-forwards:

    kubectl port-forward svc/key-server 8080
    kubectl port-forward svc/auth-server 9000
    kubectl port-forward svc/tsa 5000

and add these lines to your `/etc/hosts`

    127.0.0.1	auth-server
    127.0.0.1	key-server
    127.0.0.1	test-server

### end-to-end-security

Like the AAS Stack, but with more platform around it and a more elaborate security setup.

    stackablectl demo install -s stacks/stacks.yaml -d demos/demos.yaml -r release.yaml end-to-end-security

Also run these port-forwards:

    kubectl port-forward svc/key-server 8080
    kubectl port-forward svc/auth-server 9000
    kubectl port-forward svc/tsa 5000

and add these lines to your `/etc/hosts`

    127.0.0.1	auth-server
    127.0.0.1	key-server
    127.0.0.1	test-server
