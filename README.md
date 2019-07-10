TerasologyJRE
=============
Gradle tasks for downloading JREs, customizing them and publishing to the
[Terasology Artifactory](http://artifactory.terasology.org). These
customized JREs can be bundled along with the
[Terasology Launcher](https://github.com/MovingBlocks/TerasologyLauncher/)
to distribute it as a self-contained package that can run the launcher and
the game without requiring the user to manually install Java. 

## Features
There are downloading and publishing tasks for each of the supported
platforms. The platform names contain both the OS and its bitness:
- `Linux32`
- `Linux64`
- `Mac` (_64-bit only_)
- `Windows32`
- `Windows64`

For each of them we have the download task `downloadJreZZZ` and publishing
task `publishJreZZZPublicationToMavenRepository`, where `ZZZ` is the
target platform. To download or publish all the supported JREs, you can
use the `downloadJreAll` and `publish` tasks respectively. 

Note that for publishing to [our Artifactory](http://artifactory.terasology.org),
you need to be authorized. You can create a `gradle.properties` file in
the project root and store your credentials in this format:
```properties
# Terasology Artifactory credentials
artifactoryUser = username
artifactoryPass = password
``` 
_Note: Git is configured to ignore this file._

## Examples
#### Downloading 64-bit Linux JRE
1. Run `gradlew downloadJreLinux64`
2. The downloaded `linux64.zip` can be found inside `build/jre`

#### Downloading all JREs
1. Run `gradlew downloadJreAll`
2. All the JREs will be found inside `build/jre`

_Note: No download task will overwrite the existing zip files._

#### Publishing Mac JRE to Artifactory
1. Run `gradlew publishJreMacPublicationToMavenRepository`
2. The current version of JRE will be available [here](http://artifactory.terasology.org/artifactory/libs-release-local/org/terasology/jre/liberica/mac/)

_Note: You need to set up_ `gradle.properties` _as described previously._

#### Publishing 64-bit Windows JRE to Maven local repository
1. Run `gradlew publishJreWindows64PublicationToMavenLocal`
2. The current version of JRE will be available inside <br/>`[m2_path]/repository/org/terasology/jre/liberica/windows64/`

The value of `m2_path` depends on your OS:

|OS  |m2_path  |
|----|---------|
|Linux/Mac | `~/.m2`|
|Windows | `C:\Users\{username}\.m2`|

## Additional notes
#### Versioning
A JRE cannot be published to the Artifactory if the same version of that
JRE already exists there. So the version needs to be incremented if this
repo gets updated. This can be done by updating the global `version`
variable inside `build.gradle`. 

#### JRE vendor
We are using [BellSoft Liberica JRE](https://bell-sw.com/pages/java-8u212/)
(version 8u212) as the source of the original JRE packages. To use a
different vendor, change the respective links in `jreUrlBase` and
`jreUrlFilenames` inside `build.gradle`. Remember to update the global
`group` variable as it contains the vendor name at the end.
