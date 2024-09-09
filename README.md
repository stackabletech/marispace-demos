# Marispace Demos

## AAS demo

    stackablectl stack install -d demos/demos-v2.yaml -s stacks/stacks-v2.yaml -r stacks/gaia-x-security/release.yaml gaia-x-security

Also run these port-forwards:

    kubectl port-forward svc/key-server 8080
    kubectl port-forward svc/auth-server 9000
    kubectl port-forward svc/tsa 5000

and add these lines to your `/etc/hosts`

    127.0.0.1	auth-server
    127.0.0.1	key-server
    127.0.0.1	test-server

