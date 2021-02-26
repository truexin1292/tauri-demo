<!--
 * @Project: 
 * @Remark: 
 * @Author: 邹新许
 * @Date: 2021-02-24 11:02:34
 * @LastEditors: 邹新许
 * @LastEditTime: 2021-02-26 14:09:40
 * @FilePath: /tauri-demo/README.md
-->
# tauri-demo

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).


https://github.com/tauri-apps/tauri/edit/dev/.github/workflows/build-smoke-tests.yml

```
name: release-tauri-app

on: workflow_dispatch

jobs:
  create-and-upload-assets:
    runs-on: ${{ matrix.platform }}
    timeout-minutes: 30

    strategy:
      fail-fast: false
      matrix:
        platform: [ubuntu-latest, macos-latest, windows-latest]

    steps:
      - uses: actions/checkout@v2
      - name: setup node
        uses: actions/setup-node@v1
        with:
          node-version: 12
      - name: install rust stable
        uses: actions-rs/toolchain@v1
        with:
          toolchain: stable
          profile: minimal
      - name: install webkit2gtk (ubuntu only)
        if: matrix.platform == 'ubuntu-latest'
        run: |
          sudo apt-get update
          sudo apt-get install -y webkit2gtk-4.0
      - run: yarn
      - name: build finatr for tauri app
        run: yarn build
      - run: cargo install tauri-bundler --force
      - uses: tauri-apps/tauri-action@v0
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          includeDebug: true
          tagName: finatr-v__VERSION__
          releaseName: "Release finatr app v__VERSION__"
          releaseBody: "See the assets to download this version and install."
          releaseDraft: true
          prerelease: false

```

```
name: test on PR
on: [pull_request]

jobs:
  test:
    runs-on: ${{ matrix.platform }}

    strategy:
      fail-fast: false
      matrix:
        node-version: ['12', '14']
        platform: [ubuntu-latest, macos-latest, windows-latest]

    steps:
    - uses: actions/checkout@v2
    - uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node-version }}
    - name: Get yarn cache directory path
      id: yarn-cache-dir-path
      run: echo "::set-output name=dir::$(yarn cache dir)"
    - uses: actions/cache@v2
      with:
        path: ${{ steps.yarn-cache-dir-path.outputs.dir }}
        key: cachev1-${{ runner.os }}-yarn-${{ hashFiles('**/yarn.lock') }}
        restore-keys: |
          cachev1-${{ runner.os }}-yarn-
    - run: yarn
    - run: yarn test
    - run: yarn build

```