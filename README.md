# DalamudPluginsD17

반갑습니다! 여기는 [파이널판타지14를 위한 달라무드 플러그인 프레임워크](https://github.com/dohwacorp/Dalamud) 리포지토리입니다.
이 리포지토리는 사람들이 직접 플러그인을 만들고, 수정하고 업로드를 편하게 하기 위해 만들어졌습니다.

## 플러그인 올리기

### 리포지토리 준비하기

- 당신의 플러그인이 공개적으로 접근 가능한 깃 리포지토리에 있는지 확인하세요(GitHub, GitLab, 인증 없이 HTTP 클로닝이 가능한 Git instance 등).
- 당신의 `.csproj`를 업데이트하세요.
	- `PropertyGroup` 안에 `<RestorePackagesWithLockFile>true</RestorePackagesWithLockFile>`를 적으세요.
	- `$(DalamudLibPath)`를 사용하세요. 참조 : <https://github.com/goatcorp/SamplePlugin/blob/master/SamplePlugin/SamplePlugin.csproj#L29-L63>
- 플러그인을 Release로 Build하고, `.csproj`와 새로 생성된 lock file을 commit하세요.

### 허용 항목

플러그인을 배포하기 전, 아래 사항들을 확인하세요:

- [가이드라인](https://dalamud.dev/plugin-development/restrictions/#what-am-i-allowed-to-do-in-my-plugin) 을 충족하나요?
- 전투 요소와 관련된 기능이 있나요? 만약 그렇다면, 순수히 정보 전달만을 하며, 그 정보들은 일반적인 플레이어들이 아는 정보인가요?
- 비공식적으로 코트 검토가 이루어졌나요?
- 설치가 잘 되나요?
- 설정 창이 (만약 있다면) 잘 작동하나요?
- 플러그인의 기본 기능이 잘 작동하나요(테스트가 쉬운 경우)?
- 분명한 기술적인 이슈가 없나요?
- JSON 파일이 올바르게 포매팅됐나요? [이런 일이 미래엔 없길 바랍니다.](https://github.com/goatcorp/DalamudPackager/issues/8)
- If it's a new plugin, is it in the testing channel and not the stable channel? We want new plugins to be in testing to make it easier for the group to test, as well as reducing the impact of any unforeseen issues.
- [Technical criteria](#기술-항목)을 충족하나요??

These criteria are intended to prevent issues for users. We're happy to work with you to get you across the line; just reach out in the Discord.

### 기술 항목

플러그인을 이 리포지토리에 submit하기 전에 해야 할 몇 가지가 있습니다. 플러그인을 좀 더 좋게 만들어 줄 거예요.
- 플러그인은 `icon.png`를 가지고 있어야 합니다. 64x64 이상, 512x512 이하의 파일이 `images/` 경로에 있어야 합니다.
- For regular ImGui windows that don't do anything special, like settings and utility windows, you should use the [Dalamud Windowing API](https://goatcorp.github.io/Dalamud/api/Dalamud.Interface.Windowing.html). It enhances windows with a few nice features, like integration into the native UI closing-order, pinning, and opacity controls. If it looks like a window, it should use the windowing API. We won't reject updates to existing plugins for this, but we encourage everyone to upgrade.
- Your plugin's version/assembly version must not be based on a timestamp or continually increasing build number. Every time your plugin is built with a specific commit, no matter the time or date, should produce the same version.

### Submitting

- Fork this repository, or use the GitHub web editor (press `.` in the repo, or press the ✏ icon on an existing manifest)
- In your fork, make `stable/(plugin name)/manifest.toml` (or `testing/live/(plugin name)/manifest.toml` - note that we prefer that new plugins go to `testing/live`, so that the wrinkles can be worked out before they go out to the wider audience). For more information, [see here](https://github.com/goatcorp/DIPs/blob/main/text/17-automated-build-and-submit-pipeline.md#guide-level-explanation).

  ```toml
  [plugin]
  repository = "https://github.com/goatcorp/SamplePlugin.git"
  commit = "765d9bb434ac99a27e9a3f2ba0a555b55fe6269d"
  owners = ["goaaats"]
  project_path = "SamplePlugin"
  changelog = "Added Herobrine"
  ```

- Place the images for your plugin in an `images` subfolder: `stable/(plugin name)/images`.
  - Please note this will be [streamlined at some point in the future](https://github.com/goatcorp/DIPs/pull/45). This has not been [implemented yet](https://github.com/goatcorp/DalamudPackager/issues/9). If you can help, we'd love to hear from you!
- Make the PR. If you're using the GitHub web editor, this will be automatic.

You'll also need to be using DalamudPackager; please check the SamplePlugin for an example. If you need help, please reach out.

## Updating your plugin

Just edit the commit hash in your manifest. Please always make your updates from a new branch, to make it cleaner for us to review.

## Rebuilding in a PR

If you want to trigger a re-build of your PR, just post a comment with the content "bleatbot, rebuild".

## Secrets

If your build process requires secrets, or you want to include a secret in your plugin, use [this page](https://goatcorp.github.io/plogon-secrets) to encrypt the secret, to be included via your manifest. It will then be made available to your plugin's MSBuild/build script via environment variables, as per the instructions on the page.

---

When submitting a plugin, please consider our [Acceptable Use Policy](<https://github.com/goatcorp/FFXIVQuickLauncher/wiki/Acceptable-Use-Policy-(Official-Plugin-Repository)>) & [Terms of Service](<https://github.com/goatcorp/FFXIVQuickLauncher/wiki/Terms-and-Conditions-of-Use-(XIVLauncher,-Dalamud-&-Official-Plugin-Repository)>), which, for example, detail the rights you need to grant us when uploading a plugin to this repository. 

Please review the [plugin adoption policy](https://github.com/goatcorp/faq/blob/main/development.md#adoption) to understand what happens if you abandon your plugin. The FAQ also provides instructions on how to submit a plugin if taking over from another developer.
