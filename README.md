# Stemin Foundations

This is a content repository for Stemin. It holds one domain: `vom/`, "Vectors of meaning".
The domain teaches the mathematics of transformers. It starts with vectors, derivatives and
probability, and it ends with attention, the full transformer and the scaling laws.

The text comes from the book "Vectors of meaning" by Valentin Radu.

## Use it

Add the address of this repository in the app, on the welcome page or with the `+`. The app
fetches the files, compiles them on your device, and keeps them there.

The host must be public, and it must send CORS headers. GitHub, GitLab and Codeberg do this.
A Forgejo or Gitea of your own needs `[cors] ENABLED = true` in its `app.ini`.

## Write it

The format is in `CONTENT-MODEL.md` in the Stemin repository. Check the repository before you
push:

```
stemin check .
```

The domain has three sections: `foundations`, `blocks` and `architecture`. Each section has its
exams in `vom/checkpoints/`.

This file is not content. The app reads only `index.md`, the directories that `order` names, and
each domain's `checkpoints/`.
