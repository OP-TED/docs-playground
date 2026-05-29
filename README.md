# docs-playground

This repository is a copy of [`OP-TED/OP-TED.github.io`](https://github.com/OP-TED/OP-TED.github.io)
(*canonical repository*) maintained by the Release Wizard of the eForms
Metadata Manager application (MDM) for testing the SDK release process
automation. It is not the production site and it is not consumed by any
downstream system.

If you are looking for the real TED Developer Docs source, please use
[`OP-TED/OP-TED.github.io`](https://github.com/OP-TED/OP-TED.github.io)
instead.

## How this repository differs from the canonical repository

Three files are replaced by the wizard each time it refreshes this
repository:

- **This README.** The canonical repository's README describes the real
  TED Developer Docs site and would mislead anyone landing here by
  accident. It is replaced by the text you are now reading so that
  visitors are warned immediately.
- **`.github/workflows/publish_site.yml`.** The canonical workflow runs
  a full Antora build of the documentation site, which requires
  `node_modules`, a `yarn.lock`, and several minutes of checkout and
  rendering. None of that is useful in a wizard test, where what
  matters is only that a workflow run progresses from *queued* to
  *running* to *completed* so the wizard's polling code can observe
  the transition. The replacement workflow is a sixty-second no-op
  that always succeeds.
- **`antora-playbook.yml`.** The canonical playbook reads documentation
  content from `OP-TED/eforms-docs`. In the playground it is rewired
  to [`OP-TED/eforms-docs-playground`](https://github.com/OP-TED/eforms-docs-playground)
  so that the test cycle stays self-contained: the branches and
  content the wizard pushes during testing live in the docs-playground
  mirror of the docs repository, and this playbook reads from there.

GitHub Pages is also not enabled on this repository, so even the stub
workflow does not publish anything that would be visible on the public
web.

## How this repository is refreshed

The MDM Release Wizard exposes a refresh button on the *Start a new
release* card. The operator may press it at any time before starting
a release test. The button opens a dialogue in which one or more
playground repositories may be selected for refresh. When this
repository is selected, the wizard force-pushes the canonical
repository's current state onto it, deletes any branches and tags
that do not exist on the canonical, and then re-applies the three
overlay files listed above (this README included).

The refresh button is shown only when the MDM instance is configured
to use playground repositories — that is, in development and other
non-production deployments. In production, where MDM is configured
against the canonical repositories, the button is not shown and the
underlying endpoints are disabled.

Because every refresh erases local state on this repository, please
do not open pull requests against it, do not push commits to it, and
do not rely on its branches, tags, or GitHub Releases as a source of
truth. The authoritative artefacts live on the canonical repository
linked above.

## Other playground repositories used for testing the SDK release process

The MDM Release Wizard maintains a small family of playground
repositories under the same `OP-TED` organisation, each mirroring the
corresponding canonical repository for testing:

- [`OP-TED/eforms-sdk-playground`](https://github.com/OP-TED/eforms-sdk-playground)
  — mirror of [`OP-TED/eforms-sdk`](https://github.com/OP-TED/eforms-sdk),
  used to test the SDK release flow itself.
- [`OP-TED/eforms-docs-playground`](https://github.com/OP-TED/eforms-docs-playground)
  — mirror of [`OP-TED/eforms-docs`](https://github.com/OP-TED/eforms-docs),
  used as the target of the wizard's documentation export step.
- [`OP-TED/eforms-sdk-analyzer-playground`](https://github.com/OP-TED/eforms-sdk-analyzer-playground)
  — mirror of [`OP-TED/eforms-sdk-analyzer`](https://github.com/OP-TED/eforms-sdk-analyzer),
  used to test the analyser release flow that runs in parallel with
  the SDK release.

Each of these repositories carries a similar README and is refreshed
by the wizard in the same way.