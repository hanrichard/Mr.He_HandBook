## NPM

- [Scripts](#scripts)
- [PeerDependency](#peer-dependency)
- [Semver](#semantic-versioning)
- [Scoped Packages](#scoped-packages)
- [Show pkg latest version](#show-pkg-latest-version)
- [Cli searching rule](#cli-searching-rule)
- [Proxy](#Proxy)
- [npm ci and lockfile](#npm-ci-and-lockfile)

## Yarn

- [Lock file and dependency update](#lock-file-and-dependency-update)

### scripts

`npm` will automatically setup `$PATH` to look into `node_modules/.bin`, so you can just run commands supplied by `dependencies` and `devDependencies` directly without a global installation of module.

package.json

```json
{
  "config": { "port": 3000 }
}
```

Access port in js

```js
console.log(process.env.npm_package_config_port);
```

### peer-dependency

[Peer Dependency mechanism](https://codingwithspike.wordpress.com/2016/01/21/dealing-with-the-deprecation-of-peerdependencies-in-npm-3/).

As of `npm v3`, `peerDependency` will not be auto-installed. All you have to do is install it manually.

### semantic-versioning

Given version `1.2.3`:

- 1 - Major a large change that breaks compatibility. If users don't adapt to a major version change, stuff won't work
- 2 - Minor a new functionality that doesn't break anything
- 3 - Patch a bugfix

By default, npm installs the latest version, and prepends a caret e.g. `^1.2.12`. This signifies that at a minimum, version `1.2.12` should be used, but any version higher than that is OK, as long as it has the same major version.

### scoped-packages

`@storybook/react` is a scoped package.

- `@storybook/react` indicates it is published by `storybook` core team.
- `@storybook/react` only has to be unique in the scope `storybook` it's published in not the entire npm registry.

### show-pkg-latest-version

```js
npm show <PKG> version
```

### cli-searching-rule

In a mono repo (managed by lerna) case, `yarn` will look up in the locations as below to find the targeted cli. Lower numbered location will be searched first before looking at higher numbered ones.

1. `packages/pkgA/node_modules/.bin`
2. `<projectRoot>/node_modules/.bin`
3. Global bin folder

### Proxy

In `package.json`, specify proxy to tell development server to proxy any unknown requests to your API server:

```json
{
  "proxy": "http://localhost:8080"
}
```

This way, when you `fetch('/api/todos')` in development, the development server will recognize that it’s not a static asset, and will proxy your request to `http://localhost:8080/api/todos` as a fallback. Conveniently, this avoids `CORS` errors during local development.

Note, it is **ONLY** working in development mode.

### NPM CI and lockfile

Use `npm ci` during ci process to ensure desired dep versions are used. It looks at lockfile and only uses `package.json` for drift detection. As a result, if it sees any difference between `package.json` and lockfile it will throw an error to terminate ci process.

```shell
$ npm ci

$ yarn install --frozen-lockfile
```

Key facts about `package.json` and lockfile.

- If you have a `package.json` and you run `npm i` we generate a `package-lock.json` from it.
- If you run `npm i` with presence of `package.json` and `package-lock.json`, the latter will never be updated, even if the `package.json` would be happy with newer versions.
- If you manually edit your `package.json` to have different version ranges for a dependency which aren't compatible with your `package-lock.json` then the latter will be updated with version ranges that are compatible with that in `package.json` when run `npm i`. Further runs of `npm i` will follow the above 2.

---

### Lock file and dependency update

- If dependencies are manually modified in a `package.json` file, yarn will only update the `yarn.lock` file the next time the yarn CLI is used to install or modify dependencies. So if modifying dependencies in `package.json`, be sure to run `yarn install` to update the `yarn.lock` file.
- `yarn upgrade` allows to upgrade all the dependencies listed in a package.json to the latest versions specified by the version ranges. Or one can use `yarn upgrade --latest` will update dependencies to the latest version ignoring version range.
