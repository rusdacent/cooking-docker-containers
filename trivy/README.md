- [Trivy demo](#trivy-demo)
  - [Install Trivy](#install-trivy)
  - [Example](#example)
    - [CVEs](#cves)
    - [Secrets](#secrets)
    - [Compliance](#compliance)
    - [Allow rules](#allow-rules)

# Trivy demo

## Install Trivy

https://aquasecurity.github.io/trivy/v0.56/getting-started/installation/

## Example

### CVEs

Запустим проверку

```
$ trivy image r0binak/mtkpi:v1.4
```

Результаты

```
. . .
тут идёт 100500 строк
. . .
```

Оставим только **CRITICAL**, что бы удобней было смотреть

```
$ trivy image --severity CRITICAL r0binak/mtkpi:v1.4
```

Результаты

```
. . .
usr/local/bin/kubescape (gobinary)

Total: 2 (CRITICAL: 2)

┌──────────────────────────┬────────────────┬──────────┬────────┬──────────────────────┬─────────────────────────────────┬────────────────────────────────────────────────────────────┐
│         Library          │ Vulnerability  │ Severity │ Status │  Installed Version   │          Fixed Version          │                           Title                            │
├──────────────────────────┼────────────────┼──────────┼────────┼──────────────────────┼─────────────────────────────────┼────────────────────────────────────────────────────────────┤
│ github.com/docker/docker │ CVE-2024-41110 │ CRITICAL │ fixed  │ v26.1.0+incompatible │ 23.0.15, 26.1.5, 27.1.1, 25.0.6 │ moby: Authz zero length regression                         │
│                          │                │          │        │                      │                                 │ https://avd.aquasec.com/nvd/cve-2024-41110                 │
├──────────────────────────┼────────────────┤          │        ├──────────────────────┼─────────────────────────────────┼────────────────────────────────────────────────────────────┤
│ stdlib                   │ CVE-2024-24790 │          │        │ 1.22.3               │ 1.21.11, 1.22.4                 │ golang: net/netip: Unexpected behavior from Is methods for │
│                          │                │          │        │                      │                                 │ IPv4-mapped IPv6 addresses                                 │
│                          │                │          │        │                      │                                 │ https://avd.aquasec.com/nvd/cve-2024-24790                 │
└──────────────────────────┴────────────────┴──────────┴────────┴──────────────────────┴─────────────────────────────────┴────────────────────────────────────────────────────────────┘
. . .
```

### Secrets

```
$ trivy image --scanners=secret r0binak/mtkpi:v1.4

. . .
/etc/ssh/ssh_host_ed25519_key (secrets)

Total: 1 (UNKNOWN: 0, LOW: 0, MEDIUM: 0, HIGH: 1, CRITICAL: 0)

HIGH: AsymmetricPrivateKey (private-key)
═════════════════════════════════════════════════════════════════════════
Asymmetric Private Key
─────────────────────────────────────────────────────────────────────────
 /etc/ssh/ssh_host_ed25519_key:1 (added by 'RUN /bin/sh -c apt-get update && DEBIAN_')
─────────────────────────────────────────────────────────────────────────
   1 [ BEGIN OPENSSH PRIVATE KEY-----**************************************************************-----END OPENSSH PRI
   2   
```

### Compliance

> [!NOTE]
> ⚠ EXPERIMENTAL
> This feature might change without preserving backwards compatibility.
> https://aquasecurity.github.io/trivy/v0.56/docs/compliance/compliance/
> 
```
$ trivy image --compliance docker-cis-1.6.0 r0binak/mtkpi:v1.4 

Summary Report for compliance: CIS Docker Community Edition Benchmark v1.6.0
┌──────┬──────────┬─────────────────────────────────────────────────────────────────────────┬────────┬────────┐
│  ID  │ Severity │                              Control Name                               │ Status │ Issues │
├──────┼──────────┼─────────────────────────────────────────────────────────────────────────┼────────┼────────┤
│ 4.1  │   HIGH   │ Ensure a user for the container has been created                        │  FAIL  │   1    │
│ 4.2  │   HIGH   │ Ensure that containers use only trusted base images (Manual)            │   -    │   -    │
│ 4.3  │   HIGH   │ Ensure unnecessary packages are not installed in the container (Manual) │   -    │   -    │
│ 4.4  │ CRITICAL │ Ensure images are scanned and rebuilt to include security patches       │  FAIL  │   9    │
│ 4.5  │   LOW    │ Ensure Content trust for Docker is Enabled (Manual)                     │   -    │   -    │
│ 4.6  │   LOW    │ Ensure HEALTHCHECK instructions have been added to the container image  │  FAIL  │   1    │
│ 4.7  │   HIGH   │ Ensure update instructions are not used alone in the Dockerfile         │  PASS  │   0    │
│ 4.8  │   HIGH   │ Ensure setuid and setgid permissions are removed in the images (Manual) │   -    │   -    │
│ 4.9  │   LOW    │ Ensure COPY is used instead of ADD                                      │  PASS  │   0    │
│ 4.10 │ CRITICAL │ Ensure secrets are not stored in Dockerfiles                            │  PASS  │   0    │
│ 4.11 │  MEDIUM  │ Ensure only verified packages are installed (Manual)                    │   -    │   -    │
│ 4.12 │  MEDIUM  │ Ensure all signed artifacts are validated (Manual)                      │   -    │   -    │
└──────┴──────────┴─────────────────────────────────────────────────────────────────────────┴────────┴────────┘
```

### Allow rules

```
$ cat trivy/test.txt 
```

```
$ trivy fs test.txt
```

```
$ cp test.txt a.txt
```

```
$ trivy fs a.txt
```
