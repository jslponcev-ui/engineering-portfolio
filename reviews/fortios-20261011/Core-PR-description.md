<!--
  You are amazing! Thanks for contributing to our project!
  Please, DO NOT DELETE ANY TEXT from this template! (unless instructed).
-->
## Breaking change

FortiOS is now configured through a config entry. Existing YAML is imported, preserving SSL verification and the consider-home grace period. Polling is fixed at 12 seconds; interval_seconds is not migrated. Keep YAML until the import succeeds and a FortiOS entry appears in Devices & services, then remove only the FortiOS block. If import fails, correct the firmware or connection settings and restart to retry; FortiOS 6.4.3 or newer is required. A repair identifies matching tracked entries in known_devices.yaml for selective manual cleanup. Enable the trackers you want and review person assignments and automation references if entity IDs change.
<!--
  If your PR contains a breaking change for existing users, it is important
  to tell them what breaks, how to make it work again and why we did this.
  This piece of text is published with the release notes, so it helps if you
  write it towards our users, not us.
  Note: Remove this section if this PR is NOT a breaking change.
-->


## Proposed change

Migrate FortiOS from DeviceScanner to a config flow and ScannerEntity platform, using one asynchronous coordinator and Home Assistant's shared aiohttp session. Trackers are identified by FortiGate serial plus master MAC, appear when first online, preserve the consider-home grace period, and become unavailable after failed scans. Setup and reauthentication validate the same FortiGate identity.

Accessible devices with unsupported firmware now produce a persistent setup error stating the 6.4.3 minimum instead of a transport retry. YAML failures explain recovery and retain the configuration until import succeeds; successful retries clear the host-specific failure repair. Regression tests cover the displayed translations, rejection before client polling, recovery after firmware correction, and failed-to-successful YAML import.

Local validation of commit 1299fb759270388a4c785d5e136c2ae444bbcfd3 on Ubuntu 26.04 / Python 3.14.5: 59 FortiOS tests with 100% statement and branch coverage (270 statements, 44 branches); 837 related tests and 4 snapshots; integration Mypy, Ruff, Pylint, hassfest, requirements, license checks and official hooks on the four changed files passed. Full local prek was interrupted after WSL connection failures and low host disk space. The separate normal global Mypy hook fails on duplicate homeassistant.util.event_type modules (.py and .pyi), outside the changed FortiOS files; check configuration is unchanged. No physical FortiGate validation has been recorded.

CI run 38010176871 passed for published head 957f0aa283db4a245f3780869945af1a02f19df0. It does not validate the new local commit; CI must be verified against the exact newly published head after human review.

Dependency: Core currently uses the published aiofortiosapi==0.1.0. Its get_detected_devices() uses mac and a freshness fallback, so it is not equivalent to the legacy master_mac query. A separate tested library patch adds get_master_mac_devices() and strict response validation. The corresponding typed Core adapter passes 59 FortiOS tests with 100% coverage against that unpublished source, plus Mypy and Ruff. This PR remains draft until the maintainer releases that API and a real published version can be pinned with regenerated requirements and new CI. There is no Git dependency or invented version pin.

- Previous dependency: https://github.com/fortinet-solutions-cse/fortiosapi
- Current dependency source: https://github.com/kimfrellsen/aiofortiosapi
- Release notes: https://github.com/kimfrellsen/aiofortiosapi/blob/main/CHANGELOG.md

Documentation for UI setup, read-only API access, migration, fixed polling and recovery is prepared on home-assistant.io next, linked to this Core PR. Its PR URL will be added after the contributor reviews and authorizes submission.
<!--
  Describe the big picture of your changes here to communicate to the
  maintainers why we should accept this pull request. If it fixes a bug
  or resolves a feature request, be sure to link to that issue in the
  additional information section.
-->


## Type of change
<!--
  What type of change does your PR introduce to Home Assistant?
  NOTE: Please, check only 1! box!
  If your PR requires multiple boxes to be checked, you'll most likely need to
  split it into multiple PRs. This makes things easier and faster to code review.
-->

- [ ] Dependency upgrade
- [ ] Bugfix (non-breaking change which fixes an issue)
- [ ] New integration (thank you!)
- [ ] New feature (which adds functionality to an existing integration)
- [ ] Deprecation (breaking change to happen in the future)
- [x] Breaking change (fix/feature causing existing functionality to break)
- [ ] Code quality improvements to existing code or addition of tests

## Additional information
<!--
  Details are important, and help maintainers processing your PR.
  Please be sure to fill out additional details, if applicable.
-->

- This PR fixes or closes issue: fixes #143028
- This PR is related to issue: 
- Link to documentation pull request: 
- Link to developer documentation pull request: 
- Link to frontend pull request: 

## Checklist
<!--
  Put an `x` in the boxes that apply. You can also fill these out after
  creating the PR. If you're unsure about any of them, don't hesitate to ask.
  We're here to help! This is simply a reminder of what we are going to look
  for before merging your code.

  AI tools are welcome, but contributors are responsible for *fully*
  understanding the code before submitting a PR. Please follow our AI policy:
  https://developers.home-assistant.io/docs/ai_policy
-->

- [ ] I understand the code I am submitting and can explain how it works.
- [ ] The code change is tested and works locally.
- [x] Local tests pass. **Your PR cannot be merged unless tests pass**
- [x] There is no commented out code in this PR.
- [ ] I have followed the [development checklist][dev-checklist]
- [ ] I have followed the [perfect PR recommendations][perfect-pr]
- [x] The code has been formatted using Ruff (`ruff format homeassistant tests`)
- [x] Tests have been added to verify that the new code works.
- [ ] Any generated code has been carefully reviewed for correctness and compliance with project standards.

If user exposed functionality or configuration variables are added/changed:

- [ ] Documentation added/updated for [www.home-assistant.io][docs-repository]

If the code communicates with devices, web services, or third-party tools:

- [ ] The [manifest file][manifest-docs] has all fields filled out correctly.  
      Updated and included derived files by running: `python3 -m script.hassfest`.
- [x] New or updated dependencies have been added to `requirements_all.txt`.  
      Updated by running `python3 -m script.gen_requirements_all`.
- [ ] For the updated dependencies a diff between library versions and ideally a link to the changelog/release notes is added to the PR description.

<!--
  This project is very active and we have a high turnover of pull requests.

  Unfortunately, the number of incoming pull requests is higher than what our
  reviewers can review and merge so there is a long backlog of pull requests
  waiting for review. You can help here!
  
  By reviewing another pull request, you will help raise the code quality of
  that pull request and the final review will be faster. This way the general
  pace of pull request reviews will go up and your wait time will go down.
  
  When picking a pull request to review, try to choose one that hasn't yet
  been reviewed.

  Thanks for helping out!
-->

To help with the load of incoming pull requests:

- [ ] I have reviewed two other [open pull requests][prs] in this repository.

[prs]: https://github.com/home-assistant/core/pulls?q=is%3Aopen+is%3Apr+-author%3A%40me+-draft%3Atrue+-label%3Awaiting-for-upstream+sort%3Acreated-desc+review%3Anone+-status%3Afailure

<!--
  Thank you for contributing <3

  Below, some useful links you could explore:
-->
[dev-checklist]: https://developers.home-assistant.io/docs/development_checklist/
[manifest-docs]: https://developers.home-assistant.io/docs/creating_integration_manifest/
[quality-scale]: https://developers.home-assistant.io/docs/integration_quality_scale_index/
[docs-repository]: https://github.com/home-assistant/home-assistant.io
[perfect-pr]: https://developers.home-assistant.io/docs/review-process/#creating-the-perfect-pr

