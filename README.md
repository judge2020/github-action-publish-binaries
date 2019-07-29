# GitHub Action for Uploading Release Artifacts


Note: this is a fork of [skx's action](https://github.com/skx/github-action-publish-binaries) that doesn't run the build step on its own. This fork is for when GitHub actions is set up to run the build as one action, then have this run after that.

---

This repository contains a simple GitHub Action implementation, which allows you to attach binaries to a new release.

There are two steps to using this action:

* Create the file `.github/main.workflow` in your repository.
  * This is where you enable the action, and specify the files to add to your release.


## Sample Configuration

This configuration uploads any file in your repository which matches the
shell-pattern `puppet-summary-*`:

```
# pushes
workflow "Handle Release" {
  on = "release"
  resolves = ["Execute"]
}

# Run the magic
action "Execute" {
  uses = "skx/github-action-publish-binaries@master"
  args = "puppet-summary-*"
  secrets = ["GITHUB_TOKEN"]
}
```

We assume that a previous action generated the file(s). For example a
go-based project might create files like this using cross-compilation:

* `puppet-summary-linux-i386`
* `puppet-summary-linux-amd64`
* `puppet-summary-darwin-i386`
* `puppet-summary-darwin-amd64`
* ....

The result should be that those binaries are uploaded shortly after you go to
the Github UI and create a new release.
