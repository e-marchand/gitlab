# 🦊 GitLab

Quick reference for managing GitLab tokens, releases, and package-hosted assets.

## Index
1. [Creating a Personal Access Token (PAT)](#creating-a-personal-access-token-pat)
2. [Creating a Release](#creating-a-release)
3. [Release Assets in GitLab (Important Difference vs GitHub)](#release-assets-in-gitlab-important-difference-vs-github)
4. [How to Attach Files to a Release (Recommended Approach)](#how-to-attach-files-to-a-release-recommended-approach)
5. [Uploading Files Using the Package Registry](#uploading-files-using-the-package-registry)
6. [GitHub vs GitLab: What "Latest Release" Really Means](#github-vs-gitlab-what-latest-release-really-means)
7. [Other pages](#others)

---

## Creating a Personal Access Token (PAT)

### Official documentation
- GitLab docs: [Personal access tokens](https://docs.gitlab.com/user/profile/personal_access_tokens/)
- Token guide: [The ultimate guide to token management at GitLab](https://about.gitlab.com/blog/the-ultimate-guide-to-token-management-at-gitlab/)

### Direct links to the token configuration page

- **GitLab.com** [Personal access tokens](https://gitlab.com/-/user_settings/personal_access_tokens)
- **Self-managed / private GitLab instance** `https://<your-gitlab-domain>/-/user_settings/personal_access_tokens`

### Steps

1. Sign in to GitLab.
2. Click your **avatar** (top right).
3. Go to **Edit profile**.
4. Open **Personal access tokens**.
5. Click **Add new token**.
6. Fill in:
 - **Token name**
 - **Expiration date** (required on recent GitLab versions)
 - **Scopes** (for example: `api`, `read_repository`)
7. Click **Create personal access token**.
8. **Copy the token immediately** — it will not be shown again.

### Alternative

- [GitLab OAuth2 API](https://docs.gitlab.com/api/oauth2/)

---

## Creating a Release

### Official documentation
- [GitLab Project releases](https://docs.gitlab.com/user/project/releases/)

### What is a release in GitLab?

A release is:
- Based on a **Git tag**
- Includes optional **release notes**
- Includes optional **assets** (**LINKS**)
- Automatically provides source code archives (`.zip`, `.tar.gz`, etc.)

### Creating a release from the Web UI

1. Open your **Project**.
2. Go to **Deploy → Releases**.
3. Click **New release**.
4. Select or create a **Tag**.
5. Optionally fill in:
 - **Release title**
 - **Release notes**
 - **Assets (links)**
6. Click **Create release**.

---

## Release Assets in GitLab (Important Difference vs GitHub)

### Key difference

**⚠️ GitLab does NOT upload files into a release.**

Instead:
- Release assets are **links (URLs)**.
- GitLab stores only the **name + URL**.
- The file must already exist somewhere.

This is different from GitHub, where files are uploaded directly to the release.

### What GitLab provides by default

Every release automatically includes **Source code** archives: `.zip`, `.tar.gz`, `.tar.bz2` generated from the Git tag.

## How to Attach Files to a Release (Recommended Approach)

### Concept

To associate a file (binary, installer, archive) with a release:

1. **Upload the file somewhere**
2. **Add a link to that file** as a release asset

The **asset name** is typically the **project name** or artifact name.

---

## Uploading Files Using the Package Registry

If you want GitLab itself to host your files, use the **Package Registry**.

### Why use the Package Registry?

- Files are stored inside GitLab
- Access is authenticated
- URLs are stable and versioned
- Perfect for associating binaries with a release tag
- Not limited like GitHub to some languages

### Official documentation

- [GitLab Package Registry](https://docs.gitlab.com/user/packages/package_registry/)
- [GitLab Generic Package Registry](https://docs.gitlab.com/user/packages/generic_packages/)

### Typical workflow

1. **Build your file** (for example: `MyComponent.zip`)
2. **Upload it to the Generic Package Registry BY code/command** 
  - look at documentation for cURL command
  - or [4D example](https://gist.github.com/e-marchand/b218af0cca0d23f9b0399f42f282221f)
4. Deploy > Package Registry to see the result
5. **Use the package URL** as a release asset link
6. **Associate it with the same Git tag**
Example package URL to download using API: `[https://<host>/api/v4/projects/<encoded-project-path>/packages/generic/MyComponent/1.0.0/MyComponent.zip](https://<host>/api/v4/projects/<encoded-project-path>/packages/generic/MyComponent/1.0.0/MyComponent.zip)`

----

## GitHub vs GitLab: What "Latest Release" Really Means

### 🔗 Official documentation

- GitHub Releases API 
  - "Get the latest release": [GitHub REST Releases](https://docs.github.com/en/rest/releases/releases#get-the-latest-release)
  - [Linking to releases](https://docs.github.com/en/repositories/releasing-projects-on-github/linking-to-releases)
  - [Managing releases](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository)
- [Releases API](https://docs.gitlab.com/ee/api/releases/)

### 🗝️ Key difference

The notion of **"latest release"** cannot be carried over directly from GitHub to GitLab.

- **GitHub**: latest is a platform-defined concept
- **GitLab**: latest is a client-side decision

### 🐙 GitHub

GitHub explicitly defines and exposes the concept of a "latest release":

- Distinguishes between stable releases and pre-releases
- "Latest release" always refers to the most recent stable (non-prerelease, non-draft) version
- This notion is first-class and can be directly consumed by external tools (e.g. package managers)

Consumers can rely on GitHub to decide which release is considered "latest" without implementing additional logic.

### 🦊 GitLab

GitLab does not have the same conceptual model:

- Does not distinguish between stable releases and pre-releases
- All releases are considered final
- No semantic notion of "latest stable release"

"Latest" simply means the most recent release by date. The permalink to the latest release is purely chronological and does not account for semantic versioning or release stability.

### 🛠️ Implication for tooling

When working with GitLab:

1. Retrieve all releases.
2. Decide how to determine "latest":
   - **Newest** by date, or
   - **Highest** semantic version (custom logic required)

This has direct consequences for automation tools and package managers, which must implement additional sorting or filtering logic.

## Others

- [Why Selecting the GitLab Host Matters](gitlab_host_path_explanation.md)