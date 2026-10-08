# TERMULSCAN Build

Production build repository for TERMULScan.

## Automatic pipeline

The source repository `jntcargo053b-wq/TERMULSCAN` runs:

1. `flutter analyze`
2. `flutter test`
3. testing APK build
4. dispatches `termulscan-production` here with the exact source commit SHA

This repository then:

1. checks out that exact TERMULSCAN commit
2. runs analyze and tests again
3. injects the production signing keystore
4. builds `TERMULScan-production.apk`
5. uploads the APK as a GitHub Actions artifact
6. publishes a GitHub Release containing the APK

No APK copying or manual file transfer is required.

## Required secrets

Configure these **repository secrets in TERMULSCAN-BUILD**:

- `TERMULSCAN_KEYSTORE_BASE64` — base64 of the production `.keystore` / `.jks`
- `TERMULSCAN_KEYSTORE_PASSWORD`
- `TERMULSCAN_KEY_ALIAS`
- `TERMULSCAN_KEY_PASSWORD`

Configure this **repository secret in TERMULSCAN**:

- `TERMULSCAN_BUILD_TOKEN` — fine-grained GitHub token that can create a repository dispatch event for `TERMULSCAN-BUILD`. GitHub documents that repository dispatch requires Contents: write for a fine-grained token. 

The production keystore is never committed to either repository.

## Manual production rebuild

Use **Actions → Build TERMULScan Production APK → Run workflow** and provide the exact TERMULSCAN commit SHA.

## Output

- GitHub Actions artifact: `TERMULScan-production-<run>`
- GitHub Release asset: `TERMULScan-production.apk`
