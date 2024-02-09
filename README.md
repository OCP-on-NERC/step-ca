# NERC Internal Certificate Authority

For generating locally trusted certificates, primarily to permit internal clusters to interact with the nerc-ocp-infra vault instance.

## Configure the `step` command line tool

1. Install the [`step` command line tool](https://smallstep.com/docs/step-cli/installation/#linux-binaries).

2. Get the fingerprint of the root certificate:

    ```
    fingerprint=$(curl -s -k https://step-ca.apps.nerc-ocp-infra.rc.fas.harvard.edu/roots.pem | step certificate fingerprint /dev/stdin)
    ```

3. Configure the client with the `step ca bootstrap` command:

    ```
    step ca bootstrap --ca-url https://step-ca.apps.nerc-ocp-infra.rc.fas.harvard.edu --fingerprint "$fingerprint"
    ```

    You should see output like:

    ```
    The root certificate has been saved in /home/larsks/.step/authorities/step-ca.apps.nerc-ocp-infra.rc.fas.harvard.edu/certs/root_ca.crt.
    The authority configuration has been saved in /home/larsks/.step/authorities/step-ca.apps.nerc-ocp-infra.rc.fas.harvard.edu/config/defaults.json.
    The profile configuration has been saved in /home/larsks/.step/profiles/step-ca.apps.nerc-ocp-infra.rc.fas.harvard.edu/config/defaults.json.
    ```

## Issue a certificate

You can issue certificates using the `step ca certficiate` command. For example, to issue a certificate for `api.nerc-ocp-test.rc.fas.harvard.edu`, you would run:

```
step ca certificate api.nerc-ocp-test.rc.fas.harvard.edu server.crt server.key
```

This will prompt you for a password:

```
✔ Provisioner: admin (JWK) [kid: Lbb59Yv-nw1pUWV400-TU7W03jfjoo33qF4eccY1jG4]
Please enter the password to decrypt the provisioner key: █
```

Use the `password` field from `nerc-ocp-infra/step-ca/step-ca-password` in the vault.

This would create `server.crt` with the certificate and `server.key` with the private key.
