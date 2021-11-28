# WARNING: This is highly experimental

At this time this repository is just hacking the contents of
<https://github.com/flathub/com.obsproject.Studio.Plugin.InputOverlay.git>
in order to package the plugin at
<https://github.com/WarmUpTill/SceneSwitcher.git>.

I am the author of neither, and I have zero experience with
packaging for Flatpak.

This repository might not be following the best practices for Flatpak
packaging of extensions, and it might be overlooking some of the
dependencies for the plugin itself.

In particular, the [official building instructions for the plugin][BUILDING.md]:

- do not recommend out-of-tree builds (which is what this current version does)
- list the following dependencies as required:
  - XTest (`libxtst-dev`)
  - XScreensaver (`libxss-dev`)
  - OpenCV (`libopencv-dev`)

Regarding the dependencies, the `flatpak-builder` manifest at the moment does
not install anything on top of the KDE SDK and the OBS runtime: I was expecting
the linker to fail, yet it does not and I end up with a package that seems to
work.
Either the dependencies are already installed, or the build system is detecting
they are missing and disabling the corresponding code.

This might be an undertested build configuration, leading to unstable results.

[BUILDING.md]: https://github.com/WarmUpTill/SceneSwitcher/blob/1.16.4/BUILDING.md

## Building and installing locally

I tested this using `flatpak-builder` v1.2.0, built from source to support YAML
manifests. Be aware that the version shipped in official Ubuntu 20.04
repositories seems to lack support for YAML manifests.

```sh
flatpak install flathub com.obsproject.Studio # likely already installed
flatpak install flathub org.kde.Sdk/x86_64/5.15-21.08 # if not already installed

git clone <this_git_repo>
cd com.obsproject.Studio.Plugin.AdvancedSceneSwitcher
flatpak-builder --force-clean build-dir com.obsproject.Studio.Plugin.AdvancedSceneSwitcher.yaml --user --install
```
