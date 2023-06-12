# Create a New Version Branch for the iOS App

When doing a new release for the iOS App like `12.x`, a new version branch must be created based on `master`. It is necessary to do this in four steps. Please set the new and former version numbers accordingly

**Step 1: Create and configure the new `12.x` branch**

1.  Create a new `12.x` branch based on latest `origin/master`
2.  Copy the `.drone.star` file from the _former_ `12.x-1` branch
    (it contains the correct branch specific setup rules and replaces the current one coming from master)
3.  In `.drone.star` set `latest_version` to `12.x` (on top in section `def main(ctx)`)
4.  In `site.yml` adjust all `-version` keys according the new and former releases
    (in section `asciidoc.attributes`)
5.  In `antora.yml` change the version from `next` to `12.x` and set, if already available, the attribute `mdm-tag-name` to the correct new tag found in https://github.com/owncloud/ios-app/releases. Note that if the tag does currently not exist, let it with the old name but you must correct it when available. Only major or minor releases are relevant, but the complete and exact tag string is necessary
6.  Run a build by entering `yarn antora-local`. No errors should occur
7.  Commit the changes and push the new `12.x` branch. **DO NOT CREATE A PR!**

**Step 2: Configure the master branch to use the new `12.x` branch**

9.  Create a new `changes_necessary_for_12.x` branch based on latest `origin/master`
10.  In `.drone.star` set `latest_version` to `12.x` (on top in section `def main(ctx)`)
12. In `site.yml` in section `asciidoc.attributes`, adjust all `-version` keys related to this repo according the new and former releases. Note if those attributes exist in other content sources, they must be set to the identical value to create consistent test builds.
12. In `antora.yml` check if the version is set to `next` and that the attribute `mdm-tag-name` is set to `master`
13. Run a build by entering `yarn antora-local`. No errors should occur
14. Commit changes and push it
15. Create a Pull Request. When CI is green, all is done correctly. Merge the PR to master.

**Step 3: Set the correct iOS build branches in the docs repo**

16. In `site.yml` of [docs](https://github.com/owncloud/docs/blob/master/site.yml) adjust the last **two** branches at `url: https://github.com/owncloud/docs-client-ios-app.git` accordingly
    (in section `content.sources.url.branches`)
17. In `site.yml` of [docs](https://github.com/owncloud/docs/blob/master/site.yml) adjust all `-version` keys in section `attributes` related to this repo according the new and former releases.

**Step 4: Protection and Renaming**

18. Go to the settings of the this repository and change the protection of the branch list (Settings > Branches) so that the `12.x` branch gets protected and the `12.x-2` branch is no longer protected.
19. Rename the `12.x-2` branch to `x_archived_12.x-2`

**Text Suggestion for Step 2**

The following text is a copy/paste suggestion for the PR in step 2, replace the branch numbers accordingly:
```
These are the changes necessary to finalize the creation of the 12.x branch.

The 12.x branch is already pushed and prepared and is included in the branch protection rules.

When 12.x (iOS-App) is finally out, the 12.x-2 branch can be archived, see step 4 in https://github.com/owncloud/docs-client-ios-app/blob/master/docs/new-version-branch.md

Note, that the 12.x branch in this repo is already created, but the `latest` pointer on the web will be set to it automatically when the tag in iOS-App repo is set. This means, that in the docs homepage, `latest` will point to 12.x-1 until the tag in iOS-App repo is set accordingly. When merging this PR, 12.x-2 will be dropped from the web but is available via pdf as usual.

Note, this PR must be merged before the 12.x tag in the iOS-App repo is set to avoid a 404 for `latest`.

Note that a PR in docs must be made to announce the 12.x branch. The docs PR must be merged AFTER this PR is merged to avoid a CI error in docs.

Before merging this PR, we should take care that 12.x-2 has all changes necessary merged as post merging the 12.x-2 pdf is fixed.

@michaelstingl @hosy fyi

@mmattel @EParzefall @phil-davis
post merging this, we need to backport all relevant changes to 12.x
```
