# Create a New Version Branch for the iOS App

When doing a new release for the iiOS App like `11.x`, a new version branch must be created based on `master`. It is necessary to do this in four steps. Please set the new and former version numbers accordingly

**Step 1: Create and configure the new `11.x` branch**

1.  Create a new `11.x` branch based on latest `origin/master`
2.  Copy the `.drone.star` file from the _former_ `11.x-1` branch
    (it contains the correct branch specific setup rules and replaces the current one coming from master)
3.  In `.drone.star` set `latest_version` to `11.x` (on top in section `def main(ctx)`)
4.  In `site.yml` adjust all `-version` keys according the new and former releases
    (in section `asciidoc.attributes`)
5.  In `antora.yml` change the version from `master` to `11.x`
6.  Run a build by entering `yarn antora-local`. No errors should occur
7.  Commit the changes and push the new `11.x` branch. **DO NOT CREATE A PR!**

**Step 2: Configure the master branch to use the new `11.x` branch**

9.  Create a new `changes_necessary_for_11.x` branch based on latest `origin/master`
10.  In `.drone.star` set `latest_version` to `11.x` (on top in section `def main(ctx)`)
11. In `site.yml` adjust all `-version` keys according the new and former releases
    (in section `asciidoc.attributes`)
12. No changes in `antora.yml` but check if the version is set to `master`
13. Run a build by entering `yarn antora-local`. No errors should occur
14. Commit changes and push it
15. Create a Pull Request. When CI is green, all is done correctly. Merge the PR to master.

**Step 3: Set the correct Desktop build branches in the docs repo**

16. In `site.yml` of [docs](https://github.com/owncloud/docs/blob/master/site.yml) adjust the last **two** branches at `url: https://github.com/owncloud/docs-client-ios-app.git` accordingly
    (in section `content.sources.url.branches`)

**Step 4: Protection and Renaming**

17. Go to the settings of the this repository and change the protection of the branch list (Settings > Branches) so that the `11.x` branch gets protected and the `11.x-2` branch is no longer protected.
18. Rename the `11.x-2` branch to `x_archived_11.x-2`

**Text Suggestion for Step 2**

The following text is a copy/paste suggestion for the PR in step 2, replace the branch numbers accordingly:
```
These are the changes necessary to finalize the creation of the 11.x branch.

The 11.x branch is already pushed and prepared and is included in the branch protection rules.

When 11.x (iOS-App) is finally out, the 11.x-2 branch can be archived,
see step 4 in https://github.com/owncloud/docs-client-ios-app/blob/master/docs/new-version-branch.md

Note, that the 11.x branch in this repo is already created, but the `latest` pointer on the web
will be set to it automatically when the tag in Desktop is set. This means, that in the docs homepage,
`latest` will point to 11.x-1 until the tag in Desktop is set accordingly. When merging this PR,
11.x-2 will be dropped from the web but is available via pdf as usual.

Note, this PR must be merged before the 11.x tag in the iOS-App repo is set to avoid a 404 for `latest`.

Note that a PR in docs must be made to announce the 11.x branch. The docs PR must be merged AFTER this PR is merged to avoid a CI error in docs.

Before merging this PR, we should take care that 11.x-2 has all changes necessary merged as post
merging the 11.x-2 pdf is fixed.

@michaelstingl @hosy fyi

@mmattel @EParzefall @phil-davis
post merging this, we need to backport all relevant changes to 11.x
```
