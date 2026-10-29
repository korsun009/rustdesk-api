# RustDesk development workflow

- Work in our `korsun009` repository and publish changes to its own branches/default branch first.
- The maintained upstream is `alonginwind`; `lejianwen` is only the original ancestor/reference.
- Never create, reopen or submit an upstream PR unless the user explicitly requests that specific contribution. Pushing to our fork does not authorize a PR.
- Use only local `gh`/`git` for GitHub. Before each GitHub write, run `gh auth status` and verify active login is exactly `korsun009`.
- Keep `origin` pointing to our fork and `remote.pushDefault=origin`; upstream/original are fetch references with disabled push URLs. Use explicit `--repo` arguments for cross-repository inspection.
- Verify code before publishing. Production deployment is a separate action and must preserve live identities, credentials, databases and network settings.
