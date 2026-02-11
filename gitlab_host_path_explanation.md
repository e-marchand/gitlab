# Why Selecting the GitLab Host Matters

When a user provides a repository URL like:

    https://a.server/p1/p2/p3

we cannot always determine the repository path directly from the URL.

## The Problem

GitLab can be installed in two ways.

### 1) At the root of the domain

    https://a.server/

Repository example:

    https://a.server/p1/p2/p3

Project path:

    p1/p2/p3

------------------------------------------------------------------------

### 2) Under a path prefix (relative URL root)

    https://a.server/gitlab/

Repository example:

    - https://a.server/gitlab/p2/p3
    - https://a.server/gitlab/p2/p4/p5

Project path:

    - p2/p3
    - p2/p4/p5

------------------------------------------------------------------------

If a user pastes:

    https://a.server/p1/p2/p3

we cannot know whether:

-   `p1` is a group, or\
-   `p1` is the GitLab installation path.

Without knowing the GitLab base URL, the repository path is ambiguous.

------------------------------------------------------------------------

## Why Host Selection Is Required

To correctly resolve the project:

-   We must know the GitLab base URL.
-   The project path is everything after that base URL.

In other words:

Project Path = Full URL Path − GitLab Base Path

------------------------------------------------------------------------

## Recommended UX Approach

### Option 1 --- User selects GitLab host (recommended)

-   User pastes full repository URL.
-   User selects GitLab host from a dropdown.
-   The system strips the host + base path automatically.
-   The remaining path becomes the project path.

This is the most reliable and predictable solution.

------------------------------------------------------------------------

### Option 2 --- Auto-detect (best effort)

-   Try detecting `/api/v4/version`. /!\ BUT NEED TOKEN
-   If not found, try with the first path segment as prefix.
-   Suggest detected base URL to the user.
-   Let the user confirm or correct it.

Detection can help, but user confirmation is still important.

------------------------------------------------------------------------

## Conclusion

Because GitLab can be installed under a path prefix,\
the same URL structure can mean different things.

To avoid incorrect repository resolution,\
the GitLab host (including its base path) must be known or selected
explicitly.
