# Overlay testing:

```
kubectl kustomize base
kubectl kustomize overlays/dev
kubectl kustomize overlays/stage
```

# Updating image tags:

## Dev

```
pushd overlays/dev
    kustomize edit set image \
        fow-backend-for-frontend=europe-west2-docker.pkg.dev/fow-dev-1234/docker/fow-backend-for-frontend:3e7fac648cb8e4c1f5eeb2a1dc294dbc9471fb64
popd

pushd overlays/dev
    kustomize edit set image \
        fow-be=europe-west2-docker.pkg.dev/fow-dev-1234/docker/fow-be:1c5853be0709507e2be84d25e028ede9e7fa3021
popd

```

## Stage

```
pushd overlays/stage
    kustomize edit set image \
        fow-backend-for-frontend=europe-west2-docker.pkg.dev/fow-dev-1234/docker/fow-backend-for-frontend:3e7fac648cb8e4c1f5eeb2a1dc294dbc9471fb64
popd

pushd overlays/stage
    kustomize edit set image \
        fow-be=europe-west2-docker.pkg.dev/fow-dev-1234/docker/fow-be:1c5853be0709507e2be84d25e028ede9e7fa3021
popd

```

Unfortunately, kustomize does not allow specifying path to a kustomization
and you need to cd into the folder in which you want to update image tags.
See this [issue](https://github.com/kubernetes-sigs/kustomize/issues/2803) for more details.

# Promoting images from dev to stage

```
dev_images=$(yq '.images' overlays/dev/kustomization.yaml) \
    yq -i '.images = env(dev_images)' overlays/stage/kustomization.yaml

```

Docs:

https://github.com/kubernetes-sigs/kustomize/tree/master/examples/transformerconfigs#images-transformer

https://github.com/kubernetes-sigs/kustomize/blob/master/examples/image.md
