# chefdk-legacy

Docker images for [ChefDK](https://docs.chef.io/workstation/) 1.6.1 on CentOS 6 and CentOS 7, built from the original RPMs for environments that still require this legacy version.

Images are published to the GitHub Container Registry and signed with [Sigstore/cosign](https://github.com/sigstore/cosign) keyless signing.

## Images

| Tag | Base OS | Notes |
|-----|---------|-------|
| `latest`, `el7`, `1.6.1-el7`, `1.6.1-rN`, `1.6.1-rN-el7`, `el7-<sha>` | CentOS 7 | Recommended |
| `el6`, `1.6.1-el6`, `1.6.1-rN-el6`, `el6-<sha>` | CentOS 6 | Legacy |

```bash
# Always latest build
docker pull ghcr.io/non7top/chefdk:latest
docker pull ghcr.io/non7top/chefdk:el7
docker pull ghcr.io/non7top/chefdk:el6

# Pinned to a specific build revision
docker pull ghcr.io/non7top/chefdk:1.6.1-r5
docker pull ghcr.io/non7top/chefdk:1.6.1-r5-el6

# Pinned to a specific git commit
docker pull ghcr.io/non7top/chefdk:el7-abc1234
docker pull ghcr.io/non7top/chefdk:el6-abc1234
```

RPM packages are sourced from the Chef package repository:
- https://packages.chef.io/files/stable/chefdk/1.6.1/el/7/
- https://packages.chef.io/files/stable/chefdk/1.6.1/el/6/

The RPMs and their checksums are also published on the [Releases page](https://github.com/non7top/chefdk-legacy/releases).

## Included versions

```
ChefDK:     1.6.1
chef-client: 12.21.4
knife:      12.21.4
berks:      5.6.4
kitchen:    1.16.0
inspec:     1.25.1
```

## Usage

```bash
docker pull ghcr.io/non7top/chefdk:latest

# Run a one-off knife or chef command
docker run --rm ghcr.io/non7top/chefdk:latest knife --version
docker run --rm -v "$PWD:/workspace" -w /workspace ghcr.io/non7top/chefdk:latest chef exec ruby -e "puts RUBY_VERSION"
```

## Verify image signatures

Images are signed twice — legacy tag format (compatible with Kyverno ≤ 1.18) and OCI 1.1 Referrers API.

```bash
# Install cosign: https://docs.sigstore.dev/cosign/system_config/installation/

# Verify via OCI referrers (cosign v3+)
cosign verify ghcr.io/non7top/chefdk:latest \
  --certificate-identity-regexp="https://github.com/non7top/chefdk-legacy" \
  --certificate-oidc-issuer="https://token.actions.githubusercontent.com"

# Same for el6
cosign verify ghcr.io/non7top/chefdk:el6 \
  --certificate-identity-regexp="https://github.com/non7top/chefdk-legacy" \
  --certificate-oidc-issuer="https://token.actions.githubusercontent.com"
```

## Verify release artifacts

Each [GitHub Release](https://github.com/non7top/chefdk-legacy/releases) includes:

| File | Description |
|------|-------------|
| `chefdk-1.6.1-1.el7.x86_64.rpm` | RPM package for CentOS 7 |
| `chefdk-1.6.1-1.el6.x86_64.rpm` | RPM package for CentOS 6 |
| `SHA256SUMS` | SHA-256 checksums for both RPMs |
| `*.cosign.bundle` | Cosign keyless signature bundles for each file |

**Verify checksums:**

```bash
sha256sum -c SHA256SUMS
```

**Verify a file with cosign:**

```bash
cosign verify-blob \
  --bundle chefdk-1.6.1-1.el7.x86_64.rpm.cosign.bundle \
  chefdk-1.6.1-1.el7.x86_64.rpm

cosign verify-blob \
  --bundle SHA256SUMS.cosign.bundle \
  SHA256SUMS
```

## Build locally

```bash
# CentOS 7
docker build -f Dockerfile.el7 -t chefdk:el7 .

# CentOS 6
docker build -f Dockerfile.el6 -t chefdk:el6 .
```
