---
created: 2026-04-20T13:31:49 (UTC +10:00)
tags: []
source: https://www.bytesizego.com/blog/golang-internal-package
author: 
---

# The magic of the internal folder

> ## Excerpt
> The internal folder in Go is one of the few "magic" features in the Go language where if you use it, you get a bunch of benefit without having to do too much.

---
The internal folder in Go is one of the few “magic” features in the Go language where if you use it, you get a bunch of benefit without having to do too much.

In [organising a Go Module](https://go.dev/doc/modules/managing-dependencies) in the official Go documentation, they have this to say:

> Larger packages or commands may benefit from splitting off some functionality into supporting packages. Initially, it’s recommended placing such packages into a directory named `internal`; [this prevents](https://pkg.go.dev/) other modules from depending on packages we don’t necessarily want to expose and support for external uses. Since other projects cannot import code from our `internal` directory, we’re free to refactor its API and generally move things around without breaking external users.

The project structure for a package is thus:

```
<span><span>project-root-directory/</span></span>
<span><span>  internal/</span></span>
<span><span>    auth/</span></span>
<span><span>      auth.go</span></span>
<span><span>      auth_test.go</span></span>
<span><span>    hash/</span></span>
<span><span>      hash.go</span></span>
<span><span>      hash_test.go</span></span>
<span><span>  go.mod</span></span>
<span><span>  modname.go</span></span>
<span><span>  modname_test.go</span></span>
<span><span></span></span>
```

Let’s talk about this practically.

If you work at a larger company where you have a few different microservices, you’ll often find that some of these services need to share common code or utilities. However, you don’t want every piece of code to be accessible to all services or external packages. By using the internal package you are doing two things:

-   Signalling that the code within is “private” - you don’t intend for any other team or service to import it.
-   Actually prevent them from doing so - even if they want to break this rule, the Go tooling will not allow it.

You’ll see internal folders used in lots of large open source projects. Kubernetes has a bunch of them - here is [one example](https://github.com/kubernetes/kubernetes/tree/master/staging/src/k8s.io/kubectl/pkg).

## What is the downside of using the internal folder?

It’s basically the same as the benefits - it prevents you from importing code from within it. If you’re new to Go and don’t know about this “trick”, it can be quite hard to figure out as IDEs do not give you great guidance on this as part of autocomplete in my experience. It also means that if you ever decide to “promote” something to be importable, it can require a refactor.

My general advice is that if you are not sure - start with it in an internal folder. Allowing others to import your code should be a conscious choice, and you do not want it to happen by accident.
