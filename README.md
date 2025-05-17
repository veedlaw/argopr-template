# ArgoPR Template - be lazy without compromise

`argopr` is a shell tool designed to streamline the creation of GitHub Pull Requests when updating image tags in Helm chart values files.
<img width="1304" alt="argopr" src="https://github.com/user-attachments/assets/8616e435-1b70-46fd-8490-6be29860bc1a" />

Ever look at a Helm chart Pull Request with the diff being `1.2.3` to `1.2.4` and wonder...
* "Okay, but *what is my teammate acutally deploying* in this update?"
    * "Do I really have to go hunt down the original service PRs or release notes myself?"
* "How can I easily see later when *that one specific feature* actually got deployed?"


## Hold Up! This is a *Template* Repository

If you're an end-user wanting to use `argopr`, **you probably don't install it from here directly.**

## What's Inside This Template? (So I don't forget in 6 months)

* **`scripts/detect-tag-changes.template`**: The heart of the beast. This script template does the tag-diffing and link-generating magic. It has placeholders like `%%GITHUB_PULL_REQUEST_URL_PREFIX%%` that your company's setup script will fill in.
* **`scripts/argopr_function.template`**: The `argopr` shell function itself. This is what you'll actually run from your command line.

**Actual setup:**
1.  Clone this `argopr-template` repo.
2.  Replace the `%%PLACEHOLDERS%%` in `scripts/detect-tag-changes.template` with company's specific GitHub URL structures.
3.  Install the now-configured `detect-tag-changes` script into the user's `PATH`.
4.  Add the `argopr` function (from `scripts/argopr_function.template`) to their shell startup file (e.g., `~/.zshrc`).
5.  Make sure they have `gh` (GitHub CLI) and `git`!

This way everyone can get a version of `argopr` tailored to internal systems. 




**Core Components:**

* **`scripts/detect-tag-changes.template`**: A bash script template that, once configured:
    * Detects changes to image tags in your Helm `values/` directory.
    * Generates a list of corresponding upstream service Pull Requests or release tags.
    * Contains placeholders like `%%GITHUB_PULL_REQUEST_URL_PREFIX%%` and `%%GITHUB_RELEASE_URL_PREFIX%%` that must be replaced with your organization's specific URL structures during setup.
* **`scripts/argopr_function.template`**: A template containing the core logic of the `argopr` shell function. This function:
    * Uses `detect-tag-changes` to generate a PR body.
    * Creates a new GitHub Pull Request using the GitHub CLI (`gh`).
    * Automatically opens the created PR in your web browser.

**Configuration Placeholders in `scripts/detect-tag-changes.template`:**

The internal setup script will replace these:

* `%%GITHUB_PULL_REQUEST_URL_PREFIX%%`: The base URL prefix for constructing pull request links.
    * Example for public GitHub: `https://github.com/MyOrganization`
    * Example for GHE: `https://github.internal.mycompany.com/MyTeam`
* `%%GITHUB_RELEASE_URL_PREFIX%%`: The base URL prefix for constructing release tag links.
    * Often the same as the pull request prefix.

**How to Use (for Internal):**

1.  Create an internal script within your organization's tooling repository.
2.  This script should:
    * Clone this `argopr-template` repository.
    * Prompt the user for any necessary configurations or use predefined company defaults (like your internal GitHub Enterprise URL prefixes).
    * Replace the placeholders in `scripts/detect-tag-changes.template` with the actual company URLs.
    * Copy the processed `detect-tag-changes` script to a location in the user's `PATH` (e.g., `$HOME/bin`).
    * Inject the content of `scripts/argopr_function.template` (as a shell function named `argopr`) into the user's shell configuration file (e.g., `~/.zshrc`, `~/.bashrc`).
    * Ensure dependencies like `gh` (GitHub CLI) and `git` are installed.

