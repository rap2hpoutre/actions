# actions

The SocialGouv GitHub Actions. Actions designed for repos with a `.socialgouv` or a `.k8s` folder.


| Action                                                                   | usage                                                        |  target project      |
| ------------------------------------------------------------------------ | ------------------------------------------------------------ |:--------------------:|
| [k8s-manifests](#socialgouvactionsk8s-manifests)                         | Generate kubernetes manifests                                | `.k8s` folder        |
| [k8s-restore-db](#socialgouvactionsk8s-restore-db)                       | -                                                            | `.k8s` folder        |
| [k8s-manifests-debug](#socialgouvactionsk8s-manifests-debug)             | Output useful infos from your manifests                      |        `*`           |
| [k8s-deactivate](#socialgouvactionsk8s-deactivate)                       | Deactivate obsolete environments                             |        `*`           |
| [autodevops-manifests](#socialgouvactionsautodevops-manifests)           | Generate kubernetes manifests                                |        `*`           |
| [autodevops-deploy](#socialgouvactionsautodevops-deploy)                 | Deploy kubernetes manifests                                  |        `*`           |
| [harbor-build-register](#socialgouvactionsharbor-build-register)         | Build and register docker images on internal harbor registry |        `*`           |
| [autodevops](#socialgouvactionsautodevops)                               | Register and Deploy application                              | `.socialgouv` folder |
| [autodevops-build-register](#socialgouvactionsautodevops-build-register) | Build and register docker images on ghcr.io                  | `.socialgouv` folder |
| [autodevops-restore-db](#socialgouvactionsautodevops-restore-db)         | -                                                            | `.socialgouv` folder |
| [mirror-gitlab](#socialgouvactionsmirror-gitlab)                         | Push changes to GitLab                                       |        `*`           |



## `socialgouv/actions/k8s-manifests-debug`

- Display [useful informations from your kubernetes manifests](https://github.com/SocialGouv/sre-tools/tree/master/packages/parse-manifests) in action log
- Post a sticky comment in associated PR
- Outputs : `markdown`, `json`, `text` variables

```yaml
- uses: socialgouv/actions/k8s-manifests-debug
  with:
    path: kubernetes-manifests.yaml
    token: ${{ secrets.GITHUB_TOKEN }}
  env:
    RANCHER_PROJECT_ID: some-project-id # to provide a decent rancher url
```

see [.github/workflows/k8s-manifests-debug-test.yaml](.github/workflows/k8s-manifests-debug-test.yaml)

## `socialgouv/actions/autodevops`

- Deploy app/package to target environment

```yaml
- uses: SocialGouv/actions/autodevops
  with:
    project: "my_app"
    environment: dev # dev, preprod, prod
    imageName: my_product/my_app
    token: ${{ secrets.GITHUB_TOKEN }}
    kubeconfig: ${{ secrets.SOCIALGOUV_KUBE_CONFIG_DEV }}
```

## `socialgouv/actions/autodevops-build-register`

- Build docker image and register it to GHCR

```yaml
- uses: SocialGouv/actions/autodevops-build-register
  with:
    project: "my_app"
    imageName: my_product/my_app
    token: ${{ secrets.GITHUB_TOKEN }}
```

## `socialgouv/actions/k8s-manifests`

- Generate kubernetes manifests based on custom `.k8s` config

```yaml
- uses: SocialGouv/actions/autodevops-manifests
  with:
    environment: "dev"
```

## `socialgouv/actions/autodevops-manifests`

- Generate kubernetes manifests based on autodevops (`.socialgouv`) config

```yaml
- uses: SocialGouv/actions/autodevops-manifests
  with:
    environment: "dev"
```

## `socialgouv/actions/autodevops-deploy`

- Deploy application over kubernetes

```yaml
- uses: SocialGouv/actions/autodevops-deploy
  id: deploy
  with:
    environment: "dev"
    token: ${{ secrets.GITHUB_TOKEN }}
    kubeconfig: ${{ secrets.SOCIALGOUV_KUBE_CONFIG }}
```

Export main URL as `steps.deploy.outputs.url`

## `socialgouv/actions/autodevops-restore-db`

- Restore database based on autodevops (`.socialgouv`) config

```yaml
- uses: SocialGouv/actions/autodevops-restore-db
  with:
    kubeconfig: ${{ secrets.SOCIALGOUV_KUBE_CONFIG_DEV }}
```

## `socialgouv/actions/k8s-restore-db`

- Restore database based on custom `.k8s` config and a `jobs/restore`.

```yaml
- uses: SocialGouv/actions/k8s-restore-db
  with:
    kubeconfig: ${{ secrets.SOCIALGOUV_KUBE_CONFIG_DEV }}
```

## `socialgouv/actions/k8s-deactivate`

- Clean review branches whenever a pull request is closed.

```yaml
- uses: SocialGouv/actions/k8s-deactivate
  with:
    kubeconfig: ${{ secrets.SOCIALGOUV_KUBE_CONFIG_DEV }}
```
