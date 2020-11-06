
# Client app release process


## Before the release

* Scope for the release is developed on the `dev` branch
* UAT and Staging versions are built with the Azure CI setup and made available for QA and client teams to verify the implementation
* When the scope of the release is implemented and confirmed on the UAT and Staging builds, PM will notify the developers that we are going into the release and we can start the release process

## Standard release process

#### 1. Branch out from `dev` a branch called `release/{version_name}`
Format for the `version_name` is `{POQ platform version}.{App version on the platform}` - for example, if the POQ platform version used by the app is 18.0.4, and this is the second release on this platform version, the `version_name` would be: **18.0.4.2**. If the application updates platform library to 18.1, the `version_name` for the next release would be **18.1.1**. 
In special cases, PM or client can decide to use a different `version_name`.

#### 2. Push the new branch `release/{version_name}` to run the pipeline and create a Release Candidate test version
Branches which names start with the `release` will trigger a pipeline that will build a Release Candidate pointing to production Backend, and upload it to Firebase. 

#### 3. Get confirmation that the Release Candidate is ready for release
If there are any fixes needed, PRs should point to this this `release` branch. In the end of the process `master` branch will be merged to `dev`. If the release doesn’t go through in the end, any fixes on this branch must be merged to `dev`.

#### 4. Create a PR to `master` and ask the team for approval

#### 5. When approved, merge the `release` branch to `master`

#### 6. Create a Github release
On GitHub repository site, create a new GitHub release for `master` branch, with a **tag** formatted as a version name. Tag is important here as it will be used by the release pipeline as a version name for the release build.

#### 7. Run the Azure pipeline to create a release build for the Play Store
In the  Poq.Android Azure site find a release pipeline called `Generate app bundle` for your project and run the pipeline for `master` branch. 
Application folders can be found under [Pipelines > All section](https://dev.azure.com/poqstudio/Poq.Android/_build?view=folders), and the release pipeline under the **[Project] > Generate build > Generate App Bundle**.

**Note:** There is also a `Generate APK` pipeline, but app bundles should be used where possible.
 
#### 8. Collect the release build artifact and send to the PM to be uploaded to the Play Store Alpha channel
Artifact can be found in the pipeline job result - when pipeline is finished, select the finished pipeline run, select Job task Phase 1, and there should be 1 artifact produced available for download. Send the artifact to the PM.

#### 9. Merge `master` back to `dev` branch.

From this point on, PM will verify the Alpha release with the client and QA, and move it to the live release to a percentage of users or to the full rollout.

Any fixes needed for the Alpha and further releases should be developed as a `hotfix` on `master`.


## Hotfix release process

#### 1. Branch out from `master` a branch called `hotfix/{version_name}`

Format for the `version_name` is the same as in standard release process: `{POQ platform version}.{App version on the platform}`.

#### 2. Push the `hotfix` branch to the upstream.

#### 3. Implement a hotfix: branch out from `hotfix` branch, implement a hotfix and create a pull from your branch to `hotfix` branch

#### 4. When approved, merge the hotfix implementation to the `hotfix` branch
This will trigger a pipeline that will build a Release Candidate and upload it to Firebase.

#### 5. Get confirmation that the Release Candidate with the hotfix is ready for release
Any additional fixes should be done against the `hotfix` branch until the Release candidate is approved.
If the release doesn’t go through in the end, any fixes on this branch must be merged to `dev`.

#### 6. Create a PR to `master` and ask the team for approval

#### 7. When approved, merge the `hotfix` branch to `master`

#### 8. Create a Github release
Same as in the standard process.

#### 9. Run the Azure pipeline to create a release build for the Play Store
Same as in the standard process.

#### 10. Collect the release build artifact and send to the PM to be uploaded to the Play Store Alpha channel
Same as in the standard process.

#### 11. Merge `master` back to `dev` branch.
Same as in the standard process.
