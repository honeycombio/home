# Honeycomb OSS Lifecycle and Practices

Last update: some/date/here

## Background

Honeycomb has a large number of OSS repositories, and that number likely won’t be getting smaller with time. Currently, most of these repositories aren’t large, but the sum total of them can amount to a lot of work just to “keep the lights on”. One team may be responsible for several repositories, as the part of the Honeycomb product they own may be made up of many smaller codebases. A few repositories do require more care to ensure they’re in a good state over time.

The goal of this document is to establish how Honeycomb will manage the lifecycle of these repositories and ensure that this lifecycle management is made clear to anyone who interacts with them.

## Golden rule: team discretion

Before diving into the rest of this document, it’s worth noting that the lifecycle and practices of a given repository are ultimately at the discretion of the team who owns that repository. There may be circumstances unique to one teams’ repository that do not hold for another team’s repository. The contents of this document are not hard and fast rules that every team at Honeycomb strictly adheres to for every single repository.

That being said, most repositories will follow the guidance in this document.

## Repository states

We define a few states for a repository:

1. **Actively developed.** There is regular development activity in this repository, such as new features or enhancing existing features. There could be breaking behavioral changes during this time. Eventually, most repositories in this state will move to being maintained. Some large repositories may perpetually be in active development, with tags representing releases.
2. **Maintained.** The repository is not actively developed, but there is still maintenance activity, such as updating dependencies, fixing bugs, some small feature work, or tweaking the build. Issues like security vulnerabilities are always dealt with. Repositories in this state may remain maintained indefinitely, or may become Being sunset if they’re no longer relevant to Honeycomb’s business.
3. **Being sunset.** A repository is no longer maintained. The build might be stale, and dependencies might be out of date. Security vulnerabilities may be addressed, though, depending on severity. Repositories in this state are highly likely to be archived after some time. Issues and Pull Requests will be closed.
4. **Archived.** It is no longer possible to interact with this repository. It is archived with GitHub’s archival tool. There is zero activity of any sort. If the repository produces an artifact, there will be adequate messaging to let people know that this artifact shouldn’t be used.

How repositories move from state to state

If a repository is in the first or fourth state, it should be obvious to anyone. For most repositories that are **Actively developed**, there may be behavior changes (and breaking changes to an artifact produced) during this phase. Eventually, this will reach a stead state and the repository will become **Maintained**.

As a special case, there may be repositories that represent a larger codebase that is perpetually in active development. In this case, new releases come from snapshots of the repository (e.g., via a git tag or branch).

As a general rule of thumb, if a repository is **Maintained**, then it should still have some recent commit history. This history should mostly be dependency uptake, build tweaks, bug fixes, and the occasional feature enhancement. If it’s a tool or package you can use, there should also be instructions for how you can depend on that tool or package, and perhaps links to documentation. If a repository that is **Maintained** has seen no activity for several months, at the discretion of the team that owns it, it may be moved to **Being sunset**.

For a repository to be moved to **Being sunset**, there are additional factors other than activity that may be involved, depending on the repository:

* If an artifact is produced from the repository, and if that artifact is still relevant
   * Are users still depending on it? How many?
   * Is the artifact produced still relevant to Honeycomb’s business, or can it be “frozen in time” for what it is?
* Does the repository have interest from the community or industry such that they would be willing to take on active maintenance duties?

If a repository is **Being sunset**, then there will be three signs to watch out for:

1. No active commit history for some time
2. A note at the top of the project’s README that this repository is considered Being sunset, with a link to this document
3. A pinned issue explaining why this repository is Being sunset, with a link to a discussion where you can comment and contribute to a case for bringing the repository back into a **Maintained** state

After a period of a few months (the timeline is not necessarily set in stone), if there was no feedback indicating that a repository should be *maintained*, it will be **Archived**.

When a repository is **Archived**, it will typically stay archived forever. There will be no opportunity to engage on an archived repository. If the repository that is archived used to produce an artifact, that artifact will likely also be unsupported, and we’ll make sure there’s adequate messaging to let people know not to use that artifact.

There may also be a rare requirements change where a repository in a latter state gets bumped back into **Actively developed**. For example, maybe a tool created several years ago is now vitally important to a new initiative for Honeycomb. If that repository was archived, it will be unarchived and active development will resume on it. This should be a rare occurrence.

### Petitioning to unarchive a repository

There is no official process to petition to unarchive a repository. If you depend on an artifact that the codebase produces, and need to change it, please feel free to fork it and use as you see fit. If you wish for Honeycomb to perform the work you need, please contact [Honeycomb support](https://www.honeycomb.io/support/).

## Issue triage

Teams at Honeycomb often watch over a broad set of repositories: some are related to one another, and some are unrelated. Having a diverse array of repositories to maintain presents some challenges in effectively and consistently dealing with issues and categorizing them. To that end, we define some general and repository state-specific practices, but keep in mind that some repositories may have some more specific practices that are unique to that repository.

### Issue triage general practices

We will strive to have any new issue looked at by someone at Honeycomb within *1 week* of creation.

Repositories will have an issue template that helps set expectations for filing an issue.

When an issue is looked at, it will:

* Be appropriately labeled (bug with severity, feature request, etc.) and optionally acknowledged by someone at Honeycomb
* Be replied to by someone at Honeycomb if there is a repro needed
* Be replied to by someone at Honeycomb if there is more information needed to understand an issue
* Optionally, have its title renamed to more accurately describe the problem. This may be done to aid any automation in release notes generation, and for general improvements to searchability/accuracy. If we rename your issue, you’ll mention this is the reason.

We commit to being friendly and professional with you when you file an issue. We will try our best to be timely with any discussions on an issue.

If we ask for additional information, and don’t hear back within *2 weeks*, we will close the issue:

* This will be done with a message explaining why we’re closing it
* We will mention that we are happy to re-open it if more information is provided

We are proactive in keeping our issue lists “clean”, especially since teams who own multiple repositories may track issues in a more central location (such as a public or private Project Board).

In general, for us to keep an issue open, it needs to meet one or more of the following criteria:

* Has a minimal, self-contained reproduction OR be obviously incorrect behavior
* The problem described is unambiguous
* The issue is still relevant to the project and/or person who filed the issue
* The issue is a productive discussion about the repository/project (it may also be converted into a GitHub Discussion at the discretion of the team)

If these criteria are not met, we may close the issue as a part of regular issue grooming.

We may also choose to close very old issues. It’s often the case that old bugs diminish in severity and relevance with time, especially if the affected product area has evolved significantly over time. This is highly likely to coincide with a team’s decision to stop supporting a given feature or feature area, a decision typically made based on product usage data. We distinguish between two kinds of older issues to close:

1. Issues that are just plain old and unlikely to be relevant anymore. If you still feel that it is relevant to you, let us know and we’ll likely re-open it. And we’ll definitely work with you to get a pull request in that solves the problem.
2. Issues that we are closing due to age, relevance, and a business decision. This decision will often be based on product usage data. In this case, we will apply a WONTFIX label, and will neither re-open the issue nor accept a pull request that fixes it. This closing will be accompanied with a message explaining the business decision.

For all other concerns related to Issue management, please contact [Honeycomb support](https://www.honeycomb.io/support/).

### Issue triage practices with respect to repository states

Additionally, we define some specific practices in the context of a specific Repository state.

**Actively developed:**

* We will follow general practices for any issues filed
* Some issues might be closed because there’s in-progress work that obviates what is being raised

**Maintained:**

* We will follow general practices for any issues filed

**Being sunset:**

* We will likely close the issue, unless what is being raised is impactful enough to warrant us doing something about. An example might be a significant security vulnerability

**Archived:**

* It is not possible to file an issue

### We reserve the right to close issues

Not every issue makes sense to keep open anymore. Some of the reasons we we may close include (but are not limited to):

1. The repository is **Being sunset**
2. The issue filed is by design, even if it may not be immediately obvious why that is
3. The issue filed is a duplicate of another issue, even if it may not be immediately obvious why that is
4. The issue filed is a valid problem, but we won’t fix it and won’t accept a contribution to fix it due to strategic reasons (this is typically related to overall product strategy, which should already be articulated)

When closing an issue, we’ll do so respectfully and with an explanation of why it was closed, provided that the issue is not in violation of the repository’s Code of Conduct and clearly had some effort put into it. If you’re willing to spend time to thoughtfully submit an issue, then you’re owed an explanation for why it’s being closed.

## Pull request triage

Much like with issue triage, it can be difficult to apply a single set of rules or practices around how to handle contributions to one of our repositories. A similar framing around practices as with issue triage apply to pull request triage.

### Pull request triage general practices

We will strive to have any new pull request looked at by someone at Honeycomb within *1 week* of creation.

Repositories will have a Pull Request template that helps set expectations for filing an issue.

When a pull request is looked at, it will:

* Be appropriately labeled (patch, new feature, etc.)
* Be optionally reviewed all at once, partially reviewed, or commented on to acknowledge that we will review it
* Be merged if it has been reviewed, CI passes, and we’re comfortable with merging
* Be optionally updated to deal with merge conflicts
* Be optionally commented on to notify the submitter that they may need to rebase due to conflicts
* Be commented on for any follow-ups needed to be able to accept the pull request

We commit to being friendly and professional with you when you submit a pull request. We appreciate that you may have spent significant personal time to submit it to us, and we’ll do our best to recognize that in any interactions with you.

If we ask for a follow-up (more info, code changes, a need to add a test, etc.), and don’t hear back within *1 month*, we will either:

1. Take on the work ourselves, usually because it’s valuable enough for us to get it in promptly
2. Close the pull request

    * This will be done with a message explaining why we’re closing it
    * We are happy to re-open it if you’re willing to submit the follow-ups

Much like with our issue lists, we are proactive in keeping our pull request lists “clean”, especially since teams who own multiple repositories will likely track pull requests in a more central location.

We may choose to make an exception to the 1 month guideline for a Honeycomb engineer’s work-in-progress pull request. This would typically happen because the contribution is blocked on some other part of Honeycomb’s systems, and that period may last longer than a month. However, this should be rare, and any other Honeycomb engineer’s pull requests will be subject to the same practices as community contributions.

### Handling new features and larger pull requests

We ask that you don’t immediately submit new features and/or larger pull requests without an accepted issue related to your work.

For new features, if there isn’t already an issue with the feature request, please submit that first. During issue triage, we’ll get to it and approve/deny the feature request. If it’s approved, feel free to proceed with submitting a pull request!

In general, we prefer it if contributions are small and incremental in nature. This makes things much easier to review, and thus, lets us give you a quicker turnaround on getting code merged.

However, we recognize that not all contributions are small or can be reasonably broken up. For larger changes (e.g., architectural changes), we ask that you submit an issue discussing what you’d like to do and why. This can be an existing issue (such as a feature request issue you filed). We will engage with you on this according to our practices on managing issues.

If we all agree that your proposal makes sense, feel free to submit the bigger pull request. If we do not agree, please do not submit a pull request. It will be closed.

### Acceptance criteria

This section should be pretty obvious. Our general acceptance criteria are:

* All tests pass
* All code is formatted according to the formatting conventions in the repository (this should be automated by a .editorconfig file)
* All feedback from Honeycomb engineers is addressed

And that’s it! We have some additional criteria depending on the kind of pull request.

### Acceptance criteria based on the kind of pull request

**Patches/Bug fixes:**

Must have a regression test case demonstrating your patch fixed a bug OR the fix is small and obvious. This will be at the discretion of Honeycomb engineers

**New features or Feature enhancements:**

Must have a test case (or cases) demonstrating that your code addresses some edge cases that could come up. This will be at the discretion of Honeycomb engineers

**Performance improvements:**

If something is obvious, such as changing an O(n^2) routine into O(n), nothing else is needed. Usually it will be clear from your code alone.

If something is not obvious, you must provide evidence that your pull request improves performance in whatever dimension of performance you’re addressing (CPU time, memory pressure, wall-clock time, etc.). Most languages have some kind of statistical benchmarking tool, profiler, or something that can help in providing this evidence.

If you’re not sure how to provide evidence that your change improves things, don’t be shy! Feel free to still submit your PR and ask what kind of evidence we’re looking for. We will do our best to suggest the easiest way to get what we require, and in some circumstances, work with you to get your change merged if it’s really difficult to gather evidence.

**General tweaks (e.g., updating README, CI quirks, etc.):**

Nothing in particular here, aside from ensuring feedback is addressed.

### Pull request triage practices with respect to repository states

Additionally, we define some specific practices in the context of a specific Repository state.

**Actively developed:**

* We will try to follow general practices for any issues filed
* Some contributions might be closed because some in-progress work from the team would obviate or duplicate it

**Maintained:**

* We will follow general practices for any contributions submitted
* We will likely not accept larger feature requests. Smaller feature requests, especially if the implementation is not much code, are fair game.

**Being sunset:**

* We will likely not accept the contribution, unless what is being contributed is impactful enough to warrant cutting a new release (and likely moving the repository back to *Maintained* state)

**Archived:**

* It is not possible to submit a contribution

### We reserve the right to deny a contribution

Not every contribution makes sense for the repository in question. Reasons may include (but are not limited to):

1. The repository is **Being sunset**
2. A change is too large/complicated for us to maintain in the long term
3. A change would conflict with the goals of the library/tool/etc. built from the repository
4. We intend on no longer maintaining a repository soon, and would like to keep churn at a minimum

In general, we feel that the best way to avoid having a contribution denied is to ensure that expectations are clear. Especially for reasons like (2) and (3), it’s on us to communicate these nuances so that contributions aren’t misled about the state of a repository or the artifacts it produces.

When closing a pull request, we’ll do so respectfully and with an explanation of why it was closed, provided that the pull request is not in violation of the repository’s Code of Conduct and clearly had some effort put into it. If you’re willing to spend time thoughtfully submitting a contribution, you’re owed an explanation for why it’s closed.

For all other concerns related to Pull Request management, please contact Honeycomb support (https://www.honeycomb.io/support/).

## Release cadence

Many of the repositories we watch over are libraries or tools that are distributed via language-specific package managers. Over time, we’ll wish to release new versions of these libraries or tools. We will strive to release new versions in a timely manner, subject to the following criteria:

1. There was a security patch applied since the last release
2. There was one or more bug fixes merged since the last release
3. There was a new feature added since the last release
4. There have been some impactful dependency updates since the last release (e.g., dependency on awesome-networking-lib makes this library run faster now)

Generally speaking, we won’t release a new version just to have a new version (e.g., some minor dependency got bumped and it doesn’t impact the library meaningfully).

Additionally, if you feel that a new version of a library should be released, just contact us! A new GitHub issue, the pollinators community on slack, etc. Releases can sometimes be interrupt-driven, and that’s fine.

## Support policy for unsupported versions of languages and frameworks

Since Honeycomb has to support several languages (and sometimes several frameworks per language), we cannot become burdened with the cost of maintaining older and older versions of them over time. To that end, we commit to the following:

* If the version of a language (e.g., Ruby) or framework (e.g., Rails) that we build on top of is still in support, we fully support the artifacts produced from the repository that depend on those versions
* If the version of a language or framework that we build on top of is *out of support*, then we will no longer support that version so long as the repository is in *Active development* or is *Maintained*.
   * There may be a period of time where we’re still building against an unsupported version. There should be an issue that tracks upgrading to a supported version.

For all other concerns related to support for unsupported versions of languages and frameworks, please contact [Honeycomb support](https://www.honeycomb.io/support/).

## Community health files

Every Honeycomb repository that is under *Active development* or is *Maintained* will have the following [community health files](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file#supported-file-types) and more:

* CODE_OF_CONDUCT.md
* CONTRIBUTING.md
* Issue and pull request templates
* SECURITY.md
* SUPPORT.md
* RELEASE.md (describes the release process for the repo, if applicable)
* A `.editorconfig` file, if applicable, to help standardize code formatting

These will be standardized across repositories, with exception to CONTRIBUTING.md, since the contribution process for some repositories can differ wildly from repository to repository.

Repositories that are **Being sunset** or **Archived** may not have these files.

## Automation

We strive to automate as much process toil as possible. Our goal is to focus on the most important stuff and let things flow into relevant things we look at, such as GitHub Projects. This includes:

* Dependabot enabled on as many repositories as possible (with automatic labeling)
* Miscellaneous GitHub Actions to help with repository grooming, if appropriate, such as the [Stale bot](https://github.com/actions/stale).
* For project boards that are public, GitHub Actions that associate and move around issues and PRs to relevant states in that board
   * Some boards are also private and span multiple repositories. This movement won’t be visible to anyone other than Honeycomb employees unless those boards are also made public.
