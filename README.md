# My package template

This is my personal npm package template. It uses the latest and greatest tools while keeping things simple.

Here's what's included:

- [Bun](https://bun.sh/): fast package manager and task runner.
- [TypeScript](https://www.typescriptlang.org/): typed JavaScript.
- [tshy](https://github.com/isaacs/tshy/): package builder. It Just Works™️ for ESM + CommonJS exports.
- [Changesets](https://github.com/changesets/changesets): versioning, changelogs and releases.
  - Includes a GitHub Action that automates changelogs and releases.
- [ESLint](https://eslint.org/): linting.
  - Using the recommended rules from [`eslint`](https://eslint.org/docs/latest/rules/) and [`@typescript-eslint`](https://typescript-eslint.io/rules/?=recommended) + deterministic import sort.
- [Prettier](https://prettier.io/): code formatting.
- Bug and feature request forms for GitHub issues.

## Usage

The easiest way to use this template is by using `bun create`:

```
bun create DaniGuardiola/package-template my-package
```

Once initialized, make sure to follow these steps:

- [ ] Update the name, description and author in the `package.json` file.
- [ ] Update the heading in the `CHANGELOG.md` file.
- [ ] Replace the author in the `LICENSE` file.
- [ ] Replace this README's content with your own.
- [ ] Upload to GitHub.
- [ ] Set up the `NPM_TOKEN` secret for the publish action.
- [ ] Create a great package and publish it to npm! 🚀

## Contributing

Contributions are welcome, but I will need to agree to significant changes since this is, after all, my personal template. Feel free to open an issue to discuss it.
