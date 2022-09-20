# northstar-dedicated-ci

**Automatic bleeding-edge Docker images for the [Northstar](https://northstar.tf) dedicated server.**

These images are **FOR TESTING ONLY**. If you encounter problems, report it to whoever told you to use the image, or to the PR the image was built to test. Remember to mention the specific image tag you used. Do not report bugs you encounter to the main Northstar repositories or open a ticket on Discord.

If you want to run a server, you're probably looking for [northstar-dedicated](https://github.com/pg9182/northstar-dedicated).

## Features

- **Built upon pg9182's northstar-dedicated image**.
- **Efficiently overrides game files with newer versions**, only taking as much extra disk space as the modified files.
- **Self-serve builds** for faster Northstar development and easier community testing.
- **Stable versioning** and detailed build info.
- **Mod version override** to prevent pollution of global metrics.

## Usage

If you have access to the repository, you can submit build requests [here](https://github.com/pg9182/northstar-dedicated-ci/actions/workflows/build.yml) with the following parameters.

| Parameter | Description |
| --- | --- |
| **`Base Northstar version`** <br/> `ns_version` | **Required.** The [base image](https://github.com/pg9182/northstar-dedicated) Northstar version (without the `v` prefix). |
| **`NorthstarLauncher run ID`** <br/> `nslauncher_gharun` | Override the [NorthstarLauncher](https://github.com/R2Northstar/NorthstarLauncher) binaries with the ones from a [GitHub Actions build](https://github.com/R2Northstar/NorthstarLauncher/actions/workflows/ci.yml), referenced by the run ID (found in the URL). |
| **`NorthstarMods git ref`** <br/> `nsmods_ref` | Override the NorthstarMods with the ones from the specified [branch](https://github.com/R2Northstar/NorthstarMods/branches)/[commit](https://github.com/R2Northstar/NorthstarMods/commits). PR commits will also work. |
| **`NorthstarNavs git ref`** <br/> `nsnavs_ref` | Override the NorthstarNavs with the ones from the specified [branch](https://github.com/R2Northstar/NorthstarNavs/branches)/[commit](https://github.com/R2Northstar/NorthstarNavs/commits). PR commits will also work. |

Information about the output image tag and components will be in the build summary after it completes. Information about which files were modified/deleted/created can be found in the `Build and push` step of the build logs.

Note that the further back in time you go and the further the updated revisions differ from the base image, the less likely the final image is to work, assuming it even builds.

If the image doesn't work, you can always try creating a Northstar installation manually, then using something like `docker run --rm -it -v /path/to/northstar/installation:/northstar -e DISPLAY=xvfb -p 8081:8081/tcp -p 37015:37015/udp --entrypoint nswrap ghcr.io/pg9182/northstar-dedicated:1-tf2.0.11.0 /northstar [args]` to run it with nswrap directly (note that the usual env vars and mod merging won't take effect when used this way).

