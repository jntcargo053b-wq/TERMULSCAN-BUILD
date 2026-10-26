# Production APK signing policy

- Preserve the existing production keystore and all signing credentials.
- Never generate or substitute a new keystore for routine feature builds.
- Keep the keystore in GitHub Actions Secrets; never commit keystore files or passwords.
- Verify the APK signature before publishing.
- Publish APKs only as GitHub Release assets; do not use Actions Artifacts for distribution.
- If the configured production signing key cannot be used, stop the release and investigate rather than changing keys.
