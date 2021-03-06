---
date: 2019-02-04T17:26:52Z
title: "jx create docker"
slug: jx_create_docker
url: /commands/jx_create_docker/
---
## jx create docker

Create/update Docker auth for a given host and user in the config.json file

### Synopsis

Creates/updates an entry for secret in the Docker config.json for a given user, host

```
jx create docker auth [flags]
```

### Examples

```
  # Create/update Docker auth entry in the config.json file
  jx create docker auth --host "foo.private.docker.registry" --user "foo" --secret "FooDockerHubToken" --email "fakeemail@gmail.com"
```

### Options

```
  -b, --batch-mode                In batch mode the command never prompts for user input
  -e, --email string              The email to associate auth component of config.json
      --headless                  Enable headless operation if using browser automation
  -h, --help                      help for docker
  -t, --host string               The Docker host
      --install-dependencies      Should any required dependencies be installed automatically
      --log-level string          Logging level. Possible values - panic, fatal, error, warning, info, debug. (default "info")
      --no-brew                   Disables the use of brew on macOS to install or upgrade command line dependencies
      --pull-secrets string       The pull secrets the service account created should have (useful when deploying to your own private registry): provide multiple pull secrets by providing them in a singular block of quotes e.g. --pull-secrets "foo, bar, baz"
  -s, --secret string             The secret to associate auth component of config.json
      --skip-auth-secrets-merge   Skips merging a local git auth yaml file with any pipeline secrets that are found
  -u, --user string               The user to associate auth component of config.json
      --verbose                   Enable verbose logging
```

### SEE ALSO

* [jx create](/commands/jx_create/)	 - Create a new resource

###### Auto generated by spf13/cobra on 4-Feb-2019
