# documenso

![Version: 0.0.6](https://img.shields.io/badge/Version-0.0.6-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: v1.8.1](https://img.shields.io/badge/AppVersion-v1.8.1-informational?style=flat-square)

A Helm chart Documenso for Kubernetes

### Disclaimer

I currently still developing this chart, so there must be bugs / error produced while it applied in current Infrastructure. Make sure to follow by regarding from Official Documenso Site. I'm not recommending to install for production use.

### Step to install.

1. Initially, Documenso to prepare from start need to create / put certificate, whether from CA in 3rd party. I have put one process in deployment process before documenso deployed by initialize some containers to create self-signed.

```yaml
    spec:
      initContainers:
        - name: cert-generator
          image: alpine:latest
          command:
            - sh
            - -c
            - |
              apk add --no-cache openssl &&
              mkdir -p /certs &&
              echo "Generating CA certificate..." &&
              openssl req -x509 -newkey rsa:4096 -keyout /certs/ca-key.pem -out /certs/ca-cert.pem -days 365 -nodes -subj "/CN=Documenso CA" &&

              echo "Generating Signing Key..." &&
              openssl genpkey -algorithm RSA -out /certs/signing-key.pem &&

              echo "Generating Signing Certificate Request..." &&
              openssl req -new -key /certs/signing-key.pem -out /certs/signing.csr -subj "/CN=Documenso Signing Cert" &&

              echo "Signing the Certificate with CA..." &&
              openssl x509 -req -in /certs/signing.csr -CA /certs/ca-cert.pem -CAkey /certs/ca-key.pem -CAcreateserial -out /certs/signing-cert.pem -days 365 &&

              echo "Creating PKCS#12 (.p12) format file..." &&
              openssl pkcs12 -export -out /certs/signing-cert.p12 -inkey /certs/signing-key.pem -in /certs/signing-cert.pem -password pass:{{ .Values.documenso.certPassword }} &&

              echo "Setting ownership to documenso:documenso" &&
              chown -R 1001:1001 /certs
          volumeMounts:
            - name: cert-volume
              mountPath: /certs
```

2. Fill up the Environment variables to set in the Documenso; You can fill up for the rest configuration by yourself, but initially add:

```sh
- NEXTAUTH_URL
- NEXTAUTH_SECRET
- NEXT_PUBLIC_WEBAPP_URL
- NEXT_PUBLIC_MARKETING_URL
- NEXT_PRIVATE_DATABASE_URL
- NEXT_PRIVATE_DIRECT_DATABASE_URL
- NEXT_PRIVATE_SMTP_FROM_NAME
- NEXT_PRIVATE_SMTP_FROM_ADDRESS
```

3. The rest, try to install the Helm Chart.

```sh
helm install documenso documenso-chart/documenso
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| documenso.affinity | object | `{}` |  |
| documenso.autoscaling.enabled | bool | `false` |  |
| documenso.autoscaling.maxReplicas | int | `100` |  |
| documenso.autoscaling.minReplicas | int | `1` |  |
| documenso.autoscaling.targetCPUUtilizationPercentage | int | `80` |  |
| documenso.certPassword | string | `""` |  |
| documenso.cm.NEXTAUTH_SECRET | string | `""` |  |
| documenso.cm.NEXTAUTH_URL | string | `""` |  |
| documenso.cm.NEXT_PRIVATE_DATABASE_URL | string | `""` |  |
| documenso.cm.NEXT_PRIVATE_DIRECT_DATABASE_URL | string | `""` |  |
| documenso.cm.NEXT_PRIVATE_ENCRYPTION_KEY | string | `""` |  |
| documenso.cm.NEXT_PRIVATE_ENCRYPTION_SECONDARY_KEY | string | `""` |  |
| documenso.cm.NEXT_PRIVATE_GOOGLE_CLIENT_ID | string | `""` |  |
| documenso.cm.NEXT_PRIVATE_GOOGLE_CLIENT_SECRET | string | `""` |  |
| documenso.cm.NEXT_PRIVATE_SIGNING_LOCAL_FILE_PATH | string | `""` |  |
| documenso.cm.NEXT_PRIVATE_SMTP_FROM_ADDRESS | string | `""` |  |
| documenso.cm.NEXT_PRIVATE_SMTP_FROM_NAME | string | `""` |  |
| documenso.cm.NEXT_PRIVATE_SMTP_HOST | string | `""` |  |
| documenso.cm.NEXT_PRIVATE_SMTP_PASS | string | `""` |  |
| documenso.cm.NEXT_PRIVATE_SMTP_PORT | string | `""` |  |
| documenso.cm.NEXT_PRIVATE_SMTP_SECURE | string | `""` |  |
| documenso.cm.NEXT_PRIVATE_SMTP_USER | string | `""` |  |
| documenso.cm.NEXT_PUBLIC_MARKETING_URL | string | `""` |  |
| documenso.cm.NEXT_PUBLIC_WEBAPP_URL | string | `""` |  |
| documenso.fullnameOverride | string | `""` |  |
| documenso.image.pullPolicy | string | `"IfNotPresent"` |  |
| documenso.image.repository | string | `"documenso/documenso"` |  |
| documenso.image.tag | string | `""` |  |
| documenso.imagePullSecrets | list | `[]` |  |
| documenso.ingress.annotations | object | `{}` |  |
| documenso.ingress.className | string | `""` |  |
| documenso.ingress.enabled | bool | `false` |  |
| documenso.ingress.hosts[0].host | string | `"chart-example.local"` |  |
| documenso.ingress.hosts[0].paths[0].path | string | `"/"` |  |
| documenso.ingress.hosts[0].paths[0].pathType | string | `"ImplementationSpecific"` |  |
| documenso.ingress.tls | list | `[]` |  |
| documenso.livenessProbe.httpGet.path | string | `"/"` |  |
| documenso.livenessProbe.httpGet.port | string | `"http"` |  |
| documenso.nameOverride | string | `""` |  |
| documenso.namespaceOverride | string | `"documenso"` |  |
| documenso.nodeSelector | object | `{}` |  |
| documenso.persistence.accessMode | string | `"ReadWriteOnce"` |  |
| documenso.persistence.annotations | list | `[]` |  |
| documenso.persistence.enabled | bool | `false` |  |
| documenso.persistence.size | string | `"1Gi"` |  |
| documenso.persistence.storageClass | string | `""` |  |
| documenso.podAnnotations | object | `{}` |  |
| documenso.podLabels | object | `{}` |  |
| documenso.podSecurityContext | object | `{}` |  |
| documenso.readinessProbe.httpGet.path | string | `"/"` |  |
| documenso.readinessProbe.httpGet.port | string | `"http"` |  |
| documenso.replicaCount | int | `1` |  |
| documenso.resources.requests.memory | string | `"256Mi"` |  |
| documenso.securityContext | object | `{}` |  |
| documenso.service.port | int | `3000` |  |
| documenso.service.type | string | `"ClusterIP"` |  |
| documenso.serviceAccount.annotations | object | `{}` |  |
| documenso.serviceAccount.automount | bool | `true` |  |
| documenso.serviceAccount.create | bool | `true` |  |
| documenso.serviceAccount.name | string | `""` |  |
| documenso.tolerations | list | `[]` |  |
| documenso.volumeMounts | list | `[]` |  |
| documenso.volumes | list | `[]` |  |

To raise an issue, please go to my github chart pages: [https://github.com/rizkyfaza20/documenso-chart](https://github.com/rizkyfaza20/documenso-chart)

---
