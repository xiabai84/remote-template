# Setup remote helm-repository

Setting up GitHub Pages to point to the `packages` folder.

From there, I can create and publish `packages` like this:

```bash
$ helm package remote-template
$ mv remote-template-1.0.0.tgz packages

$ helm repo index packages --url https://github.com/xiabai84/remote-template
$ git add -i
$ git commit -av
$ git push
```

```bash
$ helm repo add remote-template https://github.com/xiabai84/remote-template