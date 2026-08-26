# OTA Downloader & Extractor

This repository contains GitHub Actions workflows for downloading OTA firmware archives and extracting selected Android partition images.

## Extract `boot.img` and `init_boot.img` from a SourceForge archive

Run the **Extract boot and init_boot images** workflow from the repository’s **Actions** tab. Choose **Run workflow**, paste the SourceForge download URL into **SourceForge or direct URL for a `.tar.zst` OTA image archive**, and optionally change the artifact name.

For example:

```text
https://sourceforge.net/projects/rama982/files/OTA-EXTRACT/X6885-15.1.2.180-OP001PF001AZ-images.tar.zst/download
```

The workflow follows redirects, downloads the archive with retry and resume support, and streams the compressed tar file through Zstandard. It extracts only members named `boot.img` and `init_boot.img`, including when they are nested inside device or version directories, so it does not unpack the complete archive onto the runner.

The completed run publishes a short-lived artifact containing any images found, a `sha256sums.txt` checksum file, and a ZIP package named from the `output_name` input. If the archive contains neither target image, the workflow fails clearly instead of publishing an empty result. The `init_boot.img` partition is optional; an archive containing only `boot.img` is still accepted.

## Other workflows

The existing workflows remain available for payload-based OTA packages, generic downloads, and Xiaomi fastboot archives. The new SourceForge workflow is intended specifically for `.tar.zst` image archives that already contain partition image files.
