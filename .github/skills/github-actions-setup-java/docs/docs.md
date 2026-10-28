# actions/setup-java reference

> Source: https://github.com/actions/setup-java

Use this reference when adding or updating `actions/setup-java` in a GitHub Actions workflow.

## Latest stable version

Use `actions/setup-java@v6.1.0` for production workflows.

## Minimal workflow example

```yaml
steps:
  - uses: actions/checkout@v7
  - uses: actions/setup-java@v6.1.0
    with:
      distribution: temurin
      java-version: '21'
  - run: java --version
```

## Key inputs

| Input | Description | Default |
| --- | --- | --- |
| `java-version` | Version to install. Accepts major versions (`21`), semver ranges, early-access tags (`27-ea`), and `latest`. Required unless `java-version-file` is set. | |
| `java-version-file` | Path to `.java-version`, `.tool-versions`, or `.sdkmanrc`. | |
| `distribution` | Distribution keyword (see table below). Required unless reading from `.sdkmanrc` with a vendor suffix. | |
| `java-package` | `jdk` (default), `jre`, `jdk+fx`, `jre+fx`, `jdk+crac`, `jre+crac`, `jdk+jmods`, `jdk+jcef`, `jre+jcef`, `jdk+ft`, or `jre+ft`. Availability varies by distribution. | `jdk` |
| `architecture` | Package architecture, such as `x64`, `aarch64`, or `riscv64`. Supported values vary by distribution and OS; defaults to the runner architecture. | Runner architecture |
| `jdk-file` | Path to a local compressed JDK archive. Requires `distribution: jdkfile`. | |
| `check-latest` | Check remote metadata before using the runner tool cache. | `false` |
| `force-download` | Always download, bypassing the tool cache. | `false` |
| `set-default` | Add Java to `PATH` and set `JAVA_HOME`. Set `false` to install a side JDK only. | `true` |
| `verify-signature` | Verify package signatures when supported (`temurin` and `microsoft`). Explicit `true` makes verification failures fatal. | Distribution-dependent |
| `verify-signature-public-key` | Custom ASCII-armored GPG public key(s) for signature verification. | |
| `problem-matcher` | Register Java compiler and uncaught-exception problem matchers. | `true` |
| `token` | Token for fetching GitHub-hosted version manifests; useful on GHES when requests are rate-limited. | `${{ github.token }}` on GitHub.com |
| `cache` | Enable dependency caching: `maven`, `gradle`, or `sbt`. | |
| `cache-dependency-path` | Override files used for cache-key hashing (useful for monorepos). | Auto-detected |
| `cache-path` | Override package-manager dependency cache paths; supports multiple paths and exclusions. | Package-manager default |
| `cache-jdk` | Cache downloaded JDK installations between jobs. | Enabled when `cache` is set |
| `cache-read-only` | Restore caches without saving (read-only mode). | `false` |
| `server-id` | Maven repository ID for generated `settings.xml`. | `github` |
| `server-username-env-var` | Env var name holding the Maven repository username. | `GITHUB_ACTOR` |
| `server-password-env-var` | Env var name holding the Maven repository password/token. | `GITHUB_TOKEN` |
| `mvn-server-credentials` | Multiple Maven server credentials in `server-id:USERNAME_ENV:PASSWORD_ENV` format. | |
| `mvn-server-repository-origins` | Maven credential origins in `server-id:repository-origin` format; requires Maven 3.10+. | |
| `mvn-repositories` / `mvn-plugin-repositories` | Custom dependency and plugin repositories for generated Maven settings. | |
| `mvn-repositories-include-central` / `mvn-repositories-prioritize-central` | Control whether Maven Central is included and prioritized when custom repositories are configured. | `true` / `true` |
| `settings-path` / `overwrite-settings` | Location for generated `settings.xml` and whether to overwrite an existing file. | `~/.m2` / `true` |
| `gpg-private-key` | GPG private key to import for signing. | |
| `gpg-passphrase-env-var` | Env var name for the GPG passphrase. | `GPG_PASSPHRASE` |
| `mvn-toolchain-id` | Maven Toolchain ID (one per installed JDK when using multiple). | `${mvn-toolchain-vendor}_${java-version}` |
| `mvn-toolchain-vendor` | Maven Toolchain vendor. | `${distribution}` |
| `show-download-progress` | Show Maven artifact download progress (adds `-ntp` when `false`). | `false` |

### Deprecated inputs (still accepted, should be replaced)

| Old input | Replacement |
| --- | --- |
| `server-username` | `server-username-env-var` |
| `server-password` | `server-password-env-var` |
| `gpg-passphrase` | `gpg-passphrase-env-var` |
| `jdkFile` | `jdk-file` |

## Outputs

| Output | Description |
| --- | --- |
| `distribution` | Distribution installed. |
| `version` | Actual Java version installed. |
| `path` | Installation path / `JAVA_HOME`. |
| `cache-hit` | Whether an exact dependency cache match was restored. |
| `cache-primary-key` | Primary cache key computed for the package manager. |

## Supported distributions

| Keyword | Distribution |
| --- | --- |
| `temurin` | Eclipse Temurin (recommended default) |
| `microsoft` | Microsoft Build of OpenJDK |
| `corretto` | Amazon Corretto |
| `oracle` | Oracle JDK |
| `oracle-openjdk` | Oracle OpenJDK |
| `zulu` | Azul Zulu OpenJDK |
| `liberica` | BellSoft Liberica JDK |
| `liberica-nik` | BellSoft Liberica Native Image Kit |
| `graalvm` | Oracle GraalVM |
| `graalvm-community` | GraalVM Community Edition (JDK 17+) |
| `semeru` | IBM Semeru Runtime (replaces `adopt-openj9`) |
| `sapmachine` | SAP SapMachine |
| `dragonwell` | Alibaba Dragonwell |
| `kona` | Tencent Kona |
| `jetbrains` | JetBrains Runtime |
| `redhat` | Red Hat Build of OpenJDK |
| `jdkfile` | Custom local JDK archive |

> `adopt` and `adopt-hotspot` are removed. Use `temurin` instead.
> `adopt-openj9` is removed. Use `semeru` instead.

## Version syntax

| Syntax | Example |
| --- | --- |
| Major version | `21`, `25` |
| Feature or patch version | `21.0`, `21.0.4` |
| Early access | `27-ea` |
| Latest stable GA | `latest` |

Reading from a version file (`.java-version`, `.tool-versions`, `.sdkmanrc`):

```yaml
- uses: actions/setup-java@v6.1.0
  with:
    distribution: temurin
    java-version-file: .java-version
```

A `.sdkmanrc` file containing `java=21.0.5-tem` will auto-detect the `temurin` distribution.

## Caching

Enable dependency and wrapper caching by setting `cache`:

```yaml
- uses: actions/setup-java@v6.1.0
  with:
    distribution: temurin
    java-version: '21'
    cache: maven        # or gradle / sbt
```

The action manages three cache types:

| Cache | Stores | Enabled by |
| --- | --- | --- |
| Dependency cache | `~/.m2/repository`, `~/.gradle/caches`, or sbt paths | `cache: maven/gradle/sbt` |
| Wrapper caches | Maven and Gradle wrapper distributions | `cache: maven` or `gradle` |
| JDK cache | Downloaded JDK installation | Auto when `cache` is set; or `cache-jdk: true` |

Override the dependency files used for key hashing in monorepos:

```yaml
- uses: actions/setup-java@v6.1.0
  with:
    distribution: temurin
    java-version: '21'
    cache: gradle
    cache-dependency-path: |
      sub-project/*.gradle*
      sub-project/**/gradle-wrapper.properties
```

## Common workflow recipes

### Maven build with caching

```yaml
name: Build

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - uses: actions/setup-java@v6.1.0
        with:
          distribution: temurin
          java-version: '21'
          cache: maven
      - run: mvn --batch-mode verify
```

### Gradle build with caching

```yaml
name: Build

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - uses: actions/setup-java@v6.1.0
        with:
          distribution: temurin
          java-version: '21'
          cache: gradle
      - run: ./gradlew build
```

### Publishing to GitHub Packages (Maven)

```yaml
- uses: actions/setup-java@v6.1.0
  with:
    distribution: temurin
    java-version: '21'
    server-id: github
    server-username-env-var: GITHUB_ACTOR
    server-password-env-var: GITHUB_TOKEN
- run: mvn --batch-mode deploy
  env:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Publishing to Maven Central with GPG signing

```yaml
- uses: actions/setup-java@v6.1.0
  with:
    distribution: temurin
    java-version: '21'
    server-id: ossrh
    server-username-env-var: MAVEN_USERNAME
    server-password-env-var: MAVEN_PASSWORD
    gpg-private-key: ${{ secrets.MAVEN_GPG_PRIVATE_KEY }}
    gpg-passphrase-env-var: MAVEN_GPG_PASSPHRASE
- run: mvn --batch-mode deploy -P release
  env:
    MAVEN_USERNAME: ${{ secrets.OSSRH_USERNAME }}
    MAVEN_PASSWORD: ${{ secrets.OSSRH_TOKEN }}
    MAVEN_GPG_PASSPHRASE: ${{ secrets.MAVEN_GPG_PASSPHRASE }}
```

### Multiple JDKs (matrix)

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        java-version: ['17', '21', '25']
    steps:
      - uses: actions/checkout@v7
      - uses: actions/setup-java@v6.1.0
        with:
          distribution: temurin
          java-version: ${{ matrix.java-version }}
          cache: maven
      - run: mvn --batch-mode verify
```

### Two JDKs in one job with Maven Toolchains

```yaml
steps:
  - uses: actions/checkout@v7
  - uses: actions/setup-java@v6.1.0
    with:
      distribution: temurin
      java-version: |
        21
        17
      cache: maven
  - run: mvn --batch-mode verify   # runs on the first listed version (21)
```

When multiple versions are listed, `JAVA_HOME` and `PATH` are set to the first listed version. Version-specific variables such as `JAVA_HOME_21_X64` are set for all installed versions. The action writes a Maven `toolchains.xml` entry for each version.

### Install JDK without changing PATH (side JDK)

```yaml
- uses: actions/setup-java@v6.1.0
  with:
    distribution: temurin
    java-version: '17'
    set-default: false
```

`JAVA_HOME_17_X64` is set but `JAVA_HOME` and `PATH` are unchanged.

## Guidance for new projects

When a project does not yet have a CI workflow, create `.github/workflows/build.yml` with the appropriate recipe above. Recommended defaults:

- `distribution: temurin` — widely used, broad version support, available on all GitHub-hosted runner platforms.
- `java-version: '21'` — current LTS (or `'25'` once it reaches LTS status). Read from `.java-version` if the project already manages versions with SDKMAN or asdf.
- `cache: maven` or `cache: gradle` depending on the build tool used in the project.

## Updating existing workflows

Check for these common issues in existing workflows:

1. **Outdated action version** — upgrade versions `v1` through `v5` to `@v6.1.0`. Versions `v1` through `v4` are deprecated.
2. **Deprecated distribution keywords** — replace `adopt`/`adopt-hotspot` with `temurin`; replace `adopt-openj9` with `semeru`.
3. **Missing cache** — add `cache: maven` or `cache: gradle` to reduce build time.
4. **Deprecated input names** — replace `server-username`, `server-password`, `gpg-passphrase`, `jdkFile` with their current equivalents.
5. **Hard-coded patch version** — consider pinning only the major version (e.g., `'21'`) so security patches are picked up automatically, or use `check-latest: true` for floating updates.

### V6 migration notes

- V6 uses the Node 24 runtime. Self-hosted runners must be version `v2.327.1` or later.
- The legacy `adopt` and `adopt-hotspot` distributions were removed; use `temurin`. `adopt-openj9` was removed; use `semeru`.
- The old Maven input names and `jdkFile` remain accepted as deprecated aliases, but use `server-username-env-var`, `server-password-env-var`, `gpg-passphrase-env-var`, and `jdk-file`.
- Maven GPG passphrases now use `gpg.passphraseEnvName`; this requires `maven-gpg-plugin` 3.2.0 or newer.

For the complete v6.1.0 input list and distribution details, see the [official README](https://github.com/actions/setup-java/blob/v6.1.0/README.md).

## Fallback reference

For inputs not covered here, consult:
- https://github.com/actions/setup-java — official repository and full README
- https://github.com/actions/setup-java/blob/main/docs/advanced-usage.md — advanced usage including GPG, toolchains, and custom distributions
