# Helm-Cheatsheet: User Service Chart

This repo contains a user service Helm chart for practicing **Helm basics and commands**. 

## Chart: user-service
- A realistic microservice-style chart for learning Helm.

## Commands to Practice:
- helm install
- helm upgrade
- helm rollback
- helm uninstall
- helm template
- helm lint
- helm diff upgrade
- helm get all / values / manifest
- dry-run & debug

## Structure
```
user-service/
├─ Chart.yaml
├─ values.yaml
├─ templates/
│   ├─ deployment.yaml
│   ├─ service.yaml
│   ├─ _helpers.tpl
│   └─ NOTES.txt
```

---
# Helm Cheatsheet (user-service)

## Create Chart
```bash
helm create user-service
```
Creates chart structure.

## Install Chart
```bash
helm install user-service ./user-service --namespace demo --create-namespace
```
Installs chart and resources.

## Dry-Run & Debug
```bash
helm install user-service ./user-service -n demo --dry-run --debug
```
Shows what would be installed.

## Template Rendering
```bash
helm template user-service ./user-service -n demo
```
Renders manifests from templates.

## Helm Template vs Dry-Run & Debug

| Command                                      | What it does                                      | Touches Cluster? |
|-----------------------------------------------|---------------------------------------------------|------------------|
| `helm template user-service ./user-service`   | Renders manifests locally, outputs YAML           | No               |
| `helm install ... --dry-run --debug`          | Simulates install, shows rendered manifests & logs| No               |

*Both commands help preview what will be deployed, but neither creates resources on the cluster.*

## Upgrade Chart
```bash
helm upgrade user-service ./user-service -n demo --set replicaCount=3
```
Updates release with new values.

## Rollback
```bash
helm rollback user-service 1 -n demo
```
Rolls back to previous revision.

## Uninstall
```bash
helm uninstall user-service -n demo
```
Deletes all resources for release.

## Inspect Release
```bash
helm get all user-service -n demo
helm get values user-service -n demo
helm get manifest user-service -n demo
```
Shows release info, values, manifests.

## Lint Chart
```bash
helm lint ./user-service
```
Checks chart for errors.

## Diff Upgrade
```bash
helm diff upgrade user-service ./user-service -n demo --values values.yaml
```
Shows difference before upgrade.

## Templates: Conditionals & Loops
**If example:**
```yaml
{{- if .Values.ingress.enabled }}
# Ingress manifest here
{{- end }}
```
**Range example:**
```yaml
{{- range $key, $value := .Values.env }}
- name: {{ $key }}
  value: {{ $value | quote }}
{{- end }}
```