# Marispace demos

Demos that showcase various things related to Marispace and/or Gaia-X.

## Visualize subsea data

    stackablectl -s stacks/stacks.yaml -d demos/demos.yaml -r release.yaml demo install trino-subsea-data

Connect to Superset and log in with `admin:adminadmin`.

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
