# Wild Wake-Initiated Lucid Dream

<p align="center"><img src="./assets/github/Group 11.png" height="128"><br>Wild</p>

<h6 align="center">Follow me on</h6>
<p align="center">
  <a aria-label="Follow @me on Youtube" href="" target="_blank">
    <img alt="Expo on Reddit" src="https://img.shields.io/badge/App_Store-0D96F6?logo=app-store&logoColor=white" target="_blank" />
  </a>&nbsp;
  <a aria-label="Follow @me on Youtube" href="" target="_blank">
    <img alt="Expo on Reddit" src="https://img.shields.io/badge/Google_Play-414141?logo=google-play&logoColor=white" target="_blank" />
  </a>&nbsp;
  <a aria-label="Follow @me on GitHub" href="https://github.com/Taiki-jp" target="_blank">
    <img alt="me on GitHub" src="https://img.shields.io/badge/GitHub-%23121011.svg?logo=github&logoColor=white" target="_blank" />
  </a>&nbsp;
  <a aria-label="Follow @me on Youtube" href="https://www.youtube.com/channel/UCvTIigpkW_1hMLVdQEC88Kw" target="_blank">
    <img alt="Expo on Reddit" src="https://img.shields.io/badge/YouTube-%23FF0000.svg?logo=YouTube&logoColor=white" target="_blank" />
  </a>&nbsp;
  <a aria-label="Follow @me on LinkedIn" href="https://www.linkedin.com/in/taiki-s-1892671a7/" target="_blank">
    <img alt="Expo on LinkedIn" src="https://img.shields.io/badge/Linkedin-%230077B5.svg?logo=linkedin&logoColor=white" target="_blank" />
  </a>
</p>

> [!WARNING]
> Development is ongoing. Thank you for your patience—I’ll launch the first app by the end of this year!

Dream is kind of Metaverse! Wild is an app designed to enrich your dream experiences. Simply by keeping a dream diary, AI will automatically generate scenes from your dreams. It also analyzes sleep quality and supports lucid dreaming. Share your dreams with other users, chat about them, and explore a world where your dreams come to life.

# ✨　Features

There are several features.

## 💄 Enjoyable UX

| Splash | Hidable Header(Footer) | PvP | DarkMode |
| ------ | ---------------------- | --- | -------- |
| <img src="./assets/github/splash.gif" height="512"> | <img src="./assets/github/hidable.gif" height="512"> | <img src="./assets/github/pvp.gif" height="512"> | <img src="./assets/github/dark.gif" height="512"> |

<details>
<summary>Tips for developers</summary>

# デザインパターン
## Container/Presentational Component パターン
- Container Component
    - ロジックを持つコンポーネント
    - Presentational Componentをラップする
    - Reduxとの接続を行う
- Presentational Component
    - UIを持つコンポーネント
    - ロジックを持たないコンポーネント
    - Container Componentから受け取ったpropsを表示する
- 参考
    - https://zenn.dev/buyselltech/articles/9460c75b7cd8d1

# フォルダ構成

※注意
App.tsxは、`src`フォルダ内に配置されている。これはデフォルトの構成と異なるため、package.json,  main.tsを修正している
```json
{
    "main": "src/App.tsx",
}
```
```typescript

import registerRootComponent from 'expo/build/launch/registerRootComponent';
import App from './src/App';

registerRootComponent(App);
```
<pre>
.
├── README.md
├── app.json
├── babel.config.js
├── main.ts
├── package-lock.json
├── package.json
├── src
│   ├── App.tsx
│   ├── api
│   ├── assets
│   ├── components
│   ├── constants
│   ├── features
│   ├── hooks
│   ├── navigation
│   └── utils
├── tsconfig.json
└── yarn.lock
</pre>

# Formatter, Linter
- Prettier
    - 特に指定なし
- ESLint
    - v8
    - v9はeslint-config-universeと互換性がないため、v8を使用する
        - https://github.com/expo/expo/issues/28144
    - autofix
        - neovim
            - `<leader>f`
        - command
            - yarn eslint . --fix


# 開発向けTIPS

## useTheme のコンポーネント内での利用不可
useTheme をコンポーネント内で使用すると以下のワーニングが発生する。 要は条件によって使用されるかわからないコンポーネント内でフックを使えないということ。解決方法としては引数として渡せばとりあえず大丈夫
https://react.dev/warnings/invalid-hook-call-warning


## Error when starting simulator
```
Error: xcrun simctl boot 1B796AF2-867E-4CC1-A6A7-160D60626BB6 exited with non-zero code: 60 An error was encountered processing the command (domain=NSPOSIXErrorDomain, code=60): Unable to boot the Simulator. launchd failed to respond. Underlying error (domain=com.apple.SimLaunchHostService.RequestError, code=4): Failed to start launchd_sim: could not bind to session, launchd_sim may have crashed or quit responding
```
### 解消方法
```fish
rm -rf ~/Library/Developer/CoreSimulator/Caches
```

### 参考
https://zenn.dev/masakiee/articles/385438253dda99

## Context の利用

- Contextを利用することで、親コンポーネントから子コンポーネントに値を渡すことができる
    - 使わない場合はバケツリレーで値を渡す必要がある
- 使用箇所
    - API

コンテクストを使わない場合
```JSX
// Without context
const ParentComponent = () => {
  const value = "Hello, World!";
  return <ChildComponent value={value} />;
};

const ChildComponent = ({ value }) => {
  return <GrandChildComponent value={value} />;
};

const GrandChildComponent = ({ value }) => {
  return <div>{value}</div>;
};
```

コンテクストを使った場合
```JSX
// With context
const ValueContext = React.createContext<string | null>(null);

const ParentComponent = () => {
  const value = "Hello, World!";
  return (
    <ValueContext.Provider value={value}>
      <ChildComponent />
    </ValueContext.Provider>
  );
};

const ChildComponent = () => {
  return <GrandChildComponent />;
};

const GrandChildComponent = () => {
  const value = useContext(ValueContext);
  return <div>{value}</div>;
};
```

## Expo SDKのバージョンアップ
### Command
```bash
yarn add expo@latest
# or specific version if needed
yarn add expo@51
# Upgrade dependencies
npx expo install --fix
```
### Ref
- https://docs.expo.dev/workflow/upgrading-expo-sdk-walkthrough/


## eslint_d のimport error
### 現象
```
[eslint_d] Error: Failed to load parser '@typescript-eslint/parser' declared in ...
```

### 解消方法
```bash
eslint_d restart
# Restart neovim
n
```
## 全ファイルの一行目にeslint のエラーが出る時
### 解決方法
```bash
yarn global add eslint_d
```
### 参考
- https://stackoverflow.com/questions/74655190/eslint-d-eslint-error-failed-to-load-plugin-import-declared-in-eslintrc
</details>
