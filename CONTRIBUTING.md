# Contributing

Contributions of any kind are welcome: bug reports, feature requests, documentation fixes, and pull requests.

## Steps

1. Open an issue or a discussion first for larger changes, so that the direction can be agreed on.
2. Fork the repo, create a branch, and make your changes.
3. Keep your changes focused: don't mix unrelated refactors with a bug fix.
4. Make sure the code is formatted and the tests still pass:

   ```bash
   npm run format
   npm run build
   npm test
   ```

5. Open a pull request. Describe what problem it solves and how you tested it.
6. Your PR will be reviewed. Please be patient, this is a spare-time project.

## What we don't accept

- Support for paid services and any PRO / subscription / account system. betterS3 only keeps the free services (S3-compatible storage, WebDAV, Dropbox, OneDrive, Webdis) and keeps all features free.
- Changes that break end-to-end encryption compatibility with existing data.

## License

By contributing, you agree that your contributions are licensed under the [Apache License 2.0](./LICENSE), the same license as the rest of the project.
