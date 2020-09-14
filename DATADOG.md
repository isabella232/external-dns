# Datadog Internal Docs

## Build

### Local Builds

The tooling to build this image needs a bit of love for modern go environments. For that reason, I suggest you stick with:

```
$ IMAGE=registry.ddbuild.io/external-dns make build.docker
```

### Release Images

Released images are created by GitLab in the `images` pipeline: https://gitlab.ddbuild.io/DataDog/images/pipelines. To create a new released image:

1. Create a release like https://github.com/DataDog/external-dns/releases/tag/v0.5.8-dd-8
2. Go to https://github.com/DataDog/images/blob/master/external-dns/
3. Create a new directory matching your released tag without the leading `v` in `external-dns/` (i.e. `external-dns/0.5.8-dd-8`)
4. Copy in the `Dockerfile` from your release, and make a PR, and merge. (i.e. https://github.com/DataDog/images/pull/1567)
5. Run https://gitlab.ddbuild.io/DataDog/images/pipelines/new with the following parameters:
- `IMAGE_NAME=external-dns`
- `IMAGE_VERSION=0.5.8-dd-8`
- `RELEASE_STAGING=true`
- `RELEASE_PROD=true`
6. Wait...

## Deploy

Once you have created a release image, hop over to https://github.com/DataDog/k8s-platform-resources/ to update charts for whichever version of clusters you wish to deploy to

### V5

Go to https://github.com/DataDog/k8s-platform-resources/tree/master/k8s/k8s-platform-external-dns and update the version in the `values.yaml` to match your release. (Example: https://github.com/DataDog/k8s-platform-resources/pull/984)

Once your PR is landed, hop over to Spinnaker: https://spinnaker.ddbuild.io/#/projects/k8s-platform/applications/k8s-platform-external-dns/executions?pipeline=meta-deploy. Select a `Manual Execution` of `meta-deploy` with the appropriate `env`.

### V2/V3

???

### Vintage

???

## Hack

We maintain this fork with the long term vision of eventually reuniting with upstream, so any Datadog specific docsshould be kept separate from `README.md`.
