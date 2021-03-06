---
title: Status
linktitle: Status
description: Current status of Jenkins X
date: 2018-02-01
publishdate: 2018-02-01
lastmod: 2018-02-01
menu:
  docs:
    parent: "about"
    weight: 120
weight: 120
sections_weight: 120
draft: false
aliases: [/about/status]
categories: [fundamentals]
toc: true
---

This page describes any specific manual work arounds you may require above and beyond changes described in the [News section](/news/) or using [jx upgrade](/commands/jx_upgrade/) to upgrade the [CLI](/commands/jx_upgrade_cli/) or [platform](/commands/jx_upgrade_platform/)
## 1st Feb 2019: Changes to the default Nexus configuration

Anonymous access to Nexus has been disabled by default, this has implications to those running Maven based builds.  To support this, the maven settings.xml injected into each build pod needs to be modified.

This can be done automatically using:

```
jx upgrade platform --update-secrets
```

NOTE: this will regenerate the settings.xml from a defined template.

If you would prefer to apply this changes manually, edit the secret `jenkins-maven-settings`, duplicating the server block for `local-nexus`, changing the server id to `nexus` e.g. 

```
<server>
    <id>local-nexus</id>
    <username>admin</username>
    <password>%s</password>
</server>
<server>
    <id>nexus</id>
    <username>admin</username>
    <password>%s</password>
</server>
```

## 8 Jan 2019: Prow and Knative Build upgrade

There are three critical bugs with the prow based Jenkins X
https://github.com/jenkins-x/jx/issues/2539
https://github.com/jenkins-x/jx/issues/2561
https://github.com/jenkins-x/jx/issues/2544

The fixes involve upgrading to a newer version of Prow and Knative Build, the latter caused an issue when performing a traditional `jx upgrade addon` so we recommend uninstalling Knative Build first (removes Knative Build related Custom Resource Definitions) and install the latest release.  

```
jx delete addon knative-build
```

And to be extra sure it’s gone maybe do an extra:

```
helm del --purge knative-build
```

then:

```
jx upgrade cli
jx upgrade addon prow
```

But this means any existing builds or custom changes to `BuildTemplate` resources will be lost.


## 5 Jan 2019: environment git repository issue

There was a regression added a few weeks ago which led to new installations setting up invalid `exposecontroller` configuration in your `Staging/Production` git repositories. See the [issue and workaround](https://github.com/jenkins-x/jx/issues/2591#issuecomment-451516674)

Make sure that the `env/values.yaml` file for your environment git repository uses `expose:` as the key in the YAML and not `exposecontroller:` - if it uses `exposecontroller:` just edit it back to `expose:` and you should be good to go!

Also we have noticed a possible regression with helm where if you have multiple `expose:` sections in your environment `env/values.yaml` it can disable the `exposecontroller` post install helm hook which can break the creation of `Ingress` resources in your environment - if you have more than one `expose:` sections please combine them into a single entry

