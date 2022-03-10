# org.korz Parent POM

[![Maven Central](https://img.shields.io/maven-central/v/org.korz/maven-parent.svg)](https://search.maven.org/artifact/org.korz/maven-parent)

The Maven parent POM for all projects in the `org.korz` group.

## Features

This main goal of this plugin is to reduce Maven boilerplate for my new
projects. Some of this is general-purpose, but some things are specific to
the `org.korz` "organization". Thus, this POM is not intended for general
use but it may provide a point of reference on how to setup your own project.

**General Purpose:**
 * Use Maven wrapper for consistent Maven version.
 * Configure modern versions of standard and commonly used plugins via
   `<pluginManagement>`.
 * Configure publishing GitHub Pages via `maven-scm-publish-plugin`
 * Configure publishing to GitHub Packages via a profile.
 * Configure publishing to OSSRH (and thus Maven Central) via a profile.
 * Publishing credentials are loaded from `settings.xml` so they are not leaked
   in any project POM. (Including this one.)

**`org.korz` Specific:**
 * Configures `<groupId>` of course...
 * Configures `<organization>` and `<developers>` as required by OSSRH.
 * Configures `${github.owner}` property to my account.
 * Configures `<license>` which is a personal preference.

Of course all those things could be overridden if you really wanted to, but
conventional wisdom is that you should own your own parent POM.

## Publishing to GitHub

Publishing to GitHub (either Pages or Packages) requires a `<server>` named
`github` in your `settings.xml` file:

```xml
<server>
  <id>github</id>
  <username><!-- Your GitHub user account --></username>
  <password><!-- GitHub personal access token --></password>
</server>
```

To publish to GitHub Packages, your PAT needs the `write:packages` scope.

TODO: What does GitHub Pages require? Is `repo`/`public_repo` enough?

## Publish to OSSRH

Publishing to OSSRH requires a `<server>` named `ossrh` in your `settings.xml`
file:

```xml
<server>
  <id>ossrh</id>
  <username><!-- Your Sonatype Jira account --></username>
  <password><!-- Your Sonatype Jira password --></password>
</server>
```

Publishing to OSSRH will not automatically release to Maven Central. The
release must be manually closed and released in the Staging Nexus console.

## GPG Signing

GPG signing is enabled for releases because OSSRH requires it. The GPG
plugin is not explicitly configured in this POM, but by default it relies on
your `settings.xml` file.

If your GPG private key has a passphrase (and it should!), then you need a
`<server>` named `gpg.passphrase`:

```xml
<server>
  <id>gpg.passphrase</id>
  <passphrase><!-- The passphrase for your GPG private key --></passphrase>
</server>
```

Note that it's a `<passphrase>` tag, not a `<password>` tag.

There are two other properties that you may wish to set globally in your
`settings.xml`. Since you cannot define properties directly in `settings.xml`,
they must be defined in a `<profile>`:

```xml
<profile>
  <!-- The id is arbitrary -->
  <id>gpg</id>
  <activation>
    <!-- This enables the profile by default, so GPG will work by default -->
    <activeByDefault>true</activeByDefault>
  </activation>
  <properties>
    <gpg.executable><!-- If gpg is not on your PATH, you can locate it here --></gpg.executable>
    <gpg.keyname><!-- If you have multiple GPG keys, you can pick one here --></gpg.keyname>
  </properties>
</profile>
```
