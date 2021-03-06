---
title: Environment
weight: 20
menu: true
---

## GitHub Token

GoReleaser requires either a GitHub API token with the `repo` scope selected to
deploy the artifacts to GitHub **or** a GitLab API token with `api` scope.
You can create one [here](https://github.com/settings/tokens/new) for GitHub or [here](https://gitlab.com/profile/personal_access_tokens) for GitLab.

This token should be added to the environment variables as `GITHUB_TOKEN` or a `GITLAB_TOKEN` respecively.
Here is how to do it with Travis CI:
[Defining Variables in Repository Settings](https://docs.travis-ci.com/user/environment-variables/#Defining-Variables-in-Repository-Settings).

Alternatively, you can provide the GitHub/GitLab token in a file. GoReleaser will check `~/.config/goreleaser/github_token` or `~/.config/goreleaser/gitlab_token` by default, you can change that in
the `.goreleaser.yml` file:

```yaml
# .goreleaser.yml
env_files:
  # use only one or release will fail!
  github_token: ~/.path/to/my/gh_token
  gitlab_token: ~/.path/to/my/gl_token
```

**IMPORTANT**: you can define both env files, but the release process will fail
because both tokens are defined. Use only one.

## GitHub Enterprise

You can use GoReleaser with GitHub Enterprise by providing its URLs in
the `.goreleaser.yml` configuration file:

```yaml
# .goreleaser.yml
github_urls:
  api: https://git.company.com/api/v3/
  upload: https://git.company.com/api/uploads/
  download: https://git.company.com/
  # set to true if you use a self-signed certificate
  skip_tls_verify: false
```

If none are set, they default to GitHub's public URLs.

## GitLab Enterprise or private hosted

You can use GoReleaser with GitHub Enterprise by providing its URLs in
the `.goreleaser.yml` configuration file:

```yaml
# .goreleaser.yml
gitlab_urls:
  api: https://gitlab.mycompany.com/api/v4/
  download: https://gitlab.company.com
  # set to true if you use a self-signed certificate
  skip_tls_verify: false
```

If none are set, they default to GitLab's public URLs.

## The dist folder

By default, GoReleaser will create its artifacts in the `./dist` folder.
If you must, you can change it by setting it in the `.goreleaser.yml` file:

```yaml
# .goreleaser.yml
dist: another-folder-that-is-not-dist
```

## Using the `main.version`

Default wise GoReleaser sets three _ldflags_:

- `main.version`: Current Git tag (the `v` prefix is stripped) or the name of
  the snapshot, if you're using the `--snapshot` flag
- `main.commit`: Current git commit SHA
- `main.date`: Date according [RFC3339](https://golang.org/pkg/time/#pkg-constants)

You can use it in your `main.go` file:

```go
package main

import "fmt"

var (
	version = "dev"
	commit  = "none"
	date    = "unknown"
)

func main() {
  fmt.Printf("%v, commit %v, built at %v", version, commit, date)
}
```

You can override this by changing the `ldflags` option in the `build` section.
