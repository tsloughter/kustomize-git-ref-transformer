Kustomize Tag Transformer with Git Reference
==

A [kustomize](https://github.com/kubernetes-sigs/kustomize) plugin that provides a transformer for changing an image tag to the current git commit reference.

Requires [jq](https://stedolan.github.io/jq/) and [yq](http://mikefarah.github.io/yq/).

## Usage

Simplest usage:

``` shell
# clone repo somewhere
$ git clone https://github.com/tsloughter/kustomize-git-ref-transformer
$ mkdir -p ~/.config/kustomize/plugin
# link to subdirectory tsloughter in the clone of this repo
$ ln -s <path to plugin clone>/tsloughter ~/.config/kustomize/plugin/tsloughter
```

And add the transformer to your kustomize resources:

``` yaml
apiVersion: tsloughter/v1
kind: GitRefTransformer
metadata:
  name: git-ref-image-tag-transformer
argsOneLiner: myimage
```

And include the transformer in your `kustomization.yaml`

``` yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
[...]
transformers:
- gitRefTransformer.yaml
```

``` shell
$ kustomize build --enable_alpha_plugins path/to/kustomize/files
```

## Options

The transformer can optionally take a new name to give the image and if the third argument is `short` it will use a short reference for the tag.

``` yaml
apiVersion: tsloughter/v1
kind: GitRefTransformer
metadata:
  name: git-ref-image-tag-transformer
argsOneLiner: ImageName [NewImageName [short]]
```

If no new image name is given you can not set the git ref to be a short reference, so simply set the new image name to the same as the image name if you want to use a short ref with the same name.
