# Publishing

`backend/plugins/spara` in `spara-app` is the source of truth for this public
marketplace. The `Publish Spara Claude plugin` GitHub Actions workflow validates
the package and mirrors this directory to `spara-ai/spara-plugins` after a change
lands on `main`.

## Release checklist

1. Make the plugin change in `backend/plugins/spara`.
2. Bump the semantic version in both `.claude-plugin/plugin.json` and the plugin
   entry in `.claude-plugin/marketplace.json` when an already-published package
   changes.
3. Run `claude plugin validate backend/plugins/spara`.
4. Run
   `node --test backend/plugins/tests/spara/plugin-contract.test.mjs`.
5. Merge the reviewed `spara-app` pull request and confirm the publish workflow
   succeeds.
6. Test the two installation commands in [README.md](README.md) from a clean
   Claude configuration.

The workflow uses the existing Spara CI GitHub App, scoped to the public
repository. No publishing token belongs in this directory or in the public
mirror.
