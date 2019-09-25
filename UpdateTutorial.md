# Updating ftc-lib-repo
## Reference Sheet
### How to checkout an update branch
* `cd` into the relevant repository
  * Create a new branch, named something descriptive like "update-to-4.0" using `git checkout -b [insert new branch name]`
* Add, commit, and push changes to new branch
  * Add changes with `git add .`
  * Commit with `git commit -m "[insert commit message]"`
  * Push with `git push --set-upstream origin [insert new branch name]`
* When finished, merge the branch
* Draft a new release *(middle bar, alongside commits amd branches)*
  * Tag name is version number
  * Title "v"+version number, for example v3.7.0
### How to merge in GitHub
* Create pull request *(top bar when in repository view)*
* Merge pull request
* Delete merged branch
# Make sure you have the right projects
* Update (or clone) [`ftc_app`](https://github.com/ftctechnh/ftc_app)
* Update (or clone) `ftc-lib-repo`, found in the [Modular FTC](https://github.com/modular-ftc) GitHub organization
# Update ftc-lib-repo
* Checkout an update branch from the `mvn-repo` branch
* For each `.aar` or `.jar` file in `ftc_app/libs` that is also in the most recent folder of its corresponding folder within `ftc-lib-repo/org/first/ftc`, run `mvn install:install-file` as specified below
* For `.aar` files, run `mvn install:install-file -Dfile=[insert full file name here] -DgroupId=org.first.ftc -DartifactId=[insert corresponding folder name] -DlocalRepositoryPath=$(readlink -f ~/ftc-lib-repo) -Dversion=[insert ftc_app version you are updating the ftc-lib-repo to] -Dpackaging=aar` if there is a -sources.jar file as well, add this: `-Dsources=[insert full sources file name]`
* For `.jar` files, run `mvn install:install-file -Dfile=[insert full file name here] -DgroupId=org.first.ftc -DartifactId=[insert corresponding folder name] -DlocalRepositoryPath=$(readlink -f ~/ftc-lib-repo) -Dversion=[insert ftc_app version you are updating the ftc-lib-repo to] -Dpackaging=jar`
# Updating vuforia-repackaged
* Checkout an update branch
* Replace `vuforia-repackaged/library/libs/Vuforia.jar` with `ftc_app/libs/Vuforia.jar`
* Replace the `vuforia-repackaged/library/src/main/jniLibs/armeabi-v7a/libVuforia.so` with `ftc_app/libs/armeabi-v7a/libVuforia.so`
* Increment the `versionCode` near the top of the `vuforia-repackaged/library/build.gradle` file (easiest to use AndroidStudio)
* Set the `versionName` to the version of the `ftc_app` you are updating to
# Updating robotcore-repackaged
* Checkout an update branch
* Replace sources in `robotcore-repackaged/library/src/main/java` with those in the `ftc_app/libs/RobotCore-release-sources.jar` by opening the archive of sources.jar such as with 7-Zip
  * `com`,`org`, and `fi`
* Open the archive of `ftc_app/libs/RobotCore-release.aar`
  * Replace the files in `robotcore-repackaged/library/src/main/jniLibs/armeabi-v7a` with those of the same name in `ftc_app/libs/RobotCore-release.aar/jni/armeabi-v7a`
  * Replace the assets folder in `robotcore-repackaged/library/src/main` with the folder of the same name in `ftc_app/libs/RobotCore-release.arr`
  * Replace the res folder in `robotcore-repackaged/library/src/main` with the folder of the same name in `ftc_app/libs/RobotCore-release.arr`
  * Replace the AndroidManifest.xml at `robotcore-repackaged/library/src/main` with the file of the same name in `ftc_app/libs/RobotCore-release.arr`
* Increment the version number and code in the build.gradle file
* Update the dependencies to the latest releases
* Build (hammer icon in top right)
* Push the branch, **but do not merge it back into master yet**
## Updating robotcore-repackaged-lite
* Checkout an update branch with the -lite suffix from the update branch already made for the master branch
* Merge lite branch into lite update branch, **THIS WILL PROBABLY PRODUCE CONFLICTS**
* Solve merge conflicts
  * If the file has been deleted in the lite branch, and the update had modified it, usually delete it (look at what it does first)
  * `build.gradle` will be modified in both, add -lite suffix to new version number
  * For java source files that had been modified in both branches, manually look through each line, keep changes from new update, assess changes from lite branch, think "why is this changed in the lite version?"
* Commit merge solutions, you are now technically done, **BUT DON'T PUSH IT YET**
* Update dependencies to the latest releases
* Build (hammer icon in top right)
* Delete new jar files that are linked to functions removed in lite version
  * Look in assets folder
  * Look for WebServer package
  * Look for OnBotJava stuff
  * Parts of new files that reference WebServer, assets, or OnBotJava (you can use Find In Path in Android Studio)
* Amend previous commit
* Merge pull request, GitHub should be alright with the merge now
* Create release for normal and lite versions
  * Normal release tag version should be just the version number, the release title should be the version number with a v in front of it
    * Example: 4.0.0, v4.0.0
  * Lite release tag version should be the version number with -lite at the end, the release title should be the version number with a v in front of it and -lite at the end
    * Make sure the target is the lite branch
    * Example: 4.0.0-lite, v4.0.0-lite

# Updating ftc-common-repackaged
* Checkout an update branch
* Replace sources in `ftc-common-repackaged/library/src/main/java` with those in the `ftc_app/libs/FtcCommon-release-sources.jar`
  * `com` and `org`
* Open the archive of `ftc_app/libs/FtcCommon-release.aar`
  * Replace the assets folder in `ftc-common-repackaged/library/src/main` with the folder of the same name in `ftc_app/libs/FtcCommon-release.arr`
  * Replace the res folder in `ftc-common-repackaged/library/src/main` with the folder of the same name in `ftc_app/libs/FtcCommon-release.arr`
  * Replace the AndroidManifest.xml at `ftc-common-repackaged/library/src/main` with the file of the same name in `ftc_app/libs/FtcCommon-release.arr`
* Increment the version number and code in the build.gradle file
* Update the dependencies to the latest releases
* Build (hammer icon in top right), makes sure everything works
* Push the branch, **but do not merge it back into master yet**
## Updating ftc-common-repackaged-lite
* Checkout an update branch with the -lite suffix from the update branch already made for the master branch
* Merge lite branch into lite update branch, **THIS WILL PROBABLY PRODUCE CONFLICTS**
* Solve merge conflicts
  * If the file has been deleted in the lite branch, and the update had modified it, usually delete it (look at what it does first)
  * `build.gradle` will be modified in both, add -lite suffix to new version number
  * For java source files that had been modified in both branches, manually look through each line, keep changes from new update, assess changes from lite branch, think "why is this changed in the lite version?"
* Commit merge solutions, you are now technically done, **BUT DON'T PUSH IT YET**
* Update the dependencies to the latest releases
* Build (hammer icon in top right)
* Delete new jar files that are linked to functions removed in lite version
  * Look in assets folder
  * Look for WebServer package
  * Look for OnBotJava stuff
  * Parts of new files that reference WebServer, assets, or OnBotJava (you can use Find In Path in Android Studio)
* Amend previous commit
* Merge pull request, GitHub should be alright with the merge now
* Create release for normal and lite versions
  * Normal release tag version should be just the version number, the release title should be the version number with a v in front of it
    * Example: 4.0.0, v4.0.0
  * Lite release tag version should be the version number with -lite at the end, the release title should be the version number with a v in front of it and -lite at the end
    * Make sure the target is the lite branch
    * Example: 4.0.0-lite, v4.0.0-lite

# Updating robotcontroller-repackaged
* Checkout an update branch
* Replace sources in `robotcontroller-repackaged/library/src/main/java` with those in the `ftc_app/FtcRobotController/src/main`
  * `assets`, `java`, `res`, and `AndroidMainifest.xml`
* Increment the version number and code in the build.gradle file
* Update the dependencies to the latest releases
* Build (hammer icon in top right), makes sure everything works
* Push the branch, **but do not merge it back into master yet**
## Updating robotcontroller-repackaged-lite
* Checkout an update branch with the -lite suffix from the update branch already made for the master branch
* Merge lite branch into lite update branch, **THIS WILL PROBABLY PRODUCE CONFLICTS**
* Solve merge conflicts
  * If the file has been deleted in the lite branch, and the update had modified it, usually delete it (look at what it does first)
  * `build.gradle` will be modified in both, add -lite suffix to new version number
  * For java source files that had been modified in both branches, manually look through each line, keep changes from new update, assess changes from lite branch, think "why is this changed in the lite version?"
* Commit merge solutions, you are now technically done, **BUT DON'T PUSH IT YET**
* Update the dependencies to the latest releases
* Build (hammer icon in top right)
* Delete new jar files that are linked to functions removed in lite version
  * Look in assets folder
  * Look for WebServer package
  * Look for OnBotJava stuff
  * Parts of new files that reference WebServer, assets, or OnBotJava (you can use Find In Path in Android Studio)
* Amend previous commit
* Merge pull request, GitHub should be alright with the merge now
* Create release for normal and lite versions
  * Normal release tag version should be just the version number, the release title should be the version number with a v in front of it
    * Example: 4.0.0, v4.0.0
  * Lite release tag version should be the version number with -lite at the end, the release title should be the version number with a v in front of it and -lite at the end
    * Make sure the target is the lite branch
    * Example: 4.0.0-lite, v4.0.0-lite