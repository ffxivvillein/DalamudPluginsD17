# DalamudPluginsD17

반갑습니다! 여기는 [파이널판타지14를 위한 달라무드 플러그인 프레임워크](https://github.com/dohwacorp/Dalamud) 리포지토리입니다.
이 리포지토리는 사람들이 직접 플러그인을 만들고, 수정하고 업로드를 편하게 하기 위해 만들어졌습니다.

## 플러그인 올리기

### 리포지토리 준비하기

- 당신의 플러그인이 공개적으로 접근 가능한 Git 리포지토리에 있는지 확인하세요(GitHub, GitLab, 인증 없이 HTTP 클로닝이 가능한 Git instance 등).
- 당신의 `.csproj`를 업데이트하세요.
	- `PropertyGroup` 안에 `<RestorePackagesWithLockFile>true</RestorePackagesWithLockFile>`를 적으세요.
	- `$(DalamudLibPath)`를 사용하세요. 참조 : [샘플 플러그인](https://github.com/goatcorp/SamplePlugin/blob/c6a5f5fcbf8e6812f274fab6347307c0283bd6fb/SamplePlugin/Dalamud.Plugin.Bootstrap.targets#L10)
- 플러그인을 Release로 Build하고, `.csproj`와 새로 생성된 lock file을 commit하세요.

### 승인 기준

플러그인 승인 그룹이 플러그인을 확인할 때, 다음 사항을 확인합니다:

- [가이드라인(영문)](https://dalamud.dev/plugin-development/restrictions#what-am-i-allowed-to-do-in-my-plugin)을 충족하나요?
- 전투 요소와 관련된 기능이 있나요? 만약 그렇다면, 순수히 정보 전달만을 하며, 플레이어가 평소 알고 있을 정보만을 보여주는 것인가요?
- 비공식적으로 코드 검토가 이루어졌나요?
- 설치가 잘 되나요?
- 설정 창이 (만약 있다면) 잘 작동하나요?
- 플러그인의 기본 기능이 잘 작동하나요(테스트가 쉬운 경우)?
- 명백한 기술적 문제가 없나요?
- JSON 파일이 형식에 맞게 작성됐나요? [이런 일이 미래엔 없길 바랍니다.](https://github.com/goatcorp/DalamudPackager/issues/8)
- 만약 새로운 플러그인이라면, stable 채널이 아닌 testing 채널에 있습니까? 우리는 새 플러그인이 테스트하기 쉽고 예기치 않은 문제를 줄이기 위해 testing 채널에 있기를 원합니다.
- [기술 기준](#기술-기준)을 충족하나요?

이러한 기준은 사용자의 문제를 방지하기 위한 것입니다. 디스코드 서버로 오셔서 함께 작업할 수 있습니다.

### 기술 기준

플러그인을 submit 하기 전, 해야 할 일이 몇 가지 있습니다. 플러그인을 사용하기 더 좋게 만들 것입니다.
- 플러그인에는 **반드시** 512x512보다 작고 64x64보다 큰 `icon.png`가 `images/`에 있어야 합니다.
- 설정 및 유틸리티 창과 같은 일반적인 창의 경우, 반드시 [달라무드 윈도우 API](https://dalamud.dev/api/Dalamud.Interface.Windowing/)를 사용하는 것이 좋을 것입니다. 기본 UI 종료 순서, 고정 및 불투명도 조정과 같은 멋진 기능을 사용할 수 있습니다. 만약 창처럼 생겼다면, 윈도우 API를 사용해야 합니다. 이를 위해 기존 플러그인에 대한 업데이트를 거부하지는 않겠지만, 모두가 업그레이드하도록 권장합니다.
- 플러그인의 버전/어셈블리 버전은 timestamp를 기반으로 하거나 지속적으로 증가하는 build 번호를 **기반으로 해서는 안됩니다**. 플러그인이 특정 commit으로 build될 때마다 시간이나 날짜에 관계없이 동일한 버전이 생성되어야 합니다.

### 제출하기

- 이 리포지토리를 Fork 하거나, GitHub 웹 편집기를 사용합니다. (리포지토리에서 `.`를 누르거나, 기존 manifest에서 ✏ 아이콘을 누르세요.)
- Fork한 리포지토리에서, `stable/(plugin name)/manifest.toml`를 만드세요. (또는 `testing/live/(plugin name)/manifest.toml` - 새로운 플러그인이 정식으로 출시되기 전에 `testing/live`에서 성능과 오류를 개선할 수 있도록 하는 것을 선호합니다). 자세한 내용은, [여기를 참조하십시오](https://github.com/goatcorp/DIPs/blob/main/text/17-automated-build-and-submit-pipeline.md#guide-level-explanation).

  ```toml
  [plugin]
  repository = "https://github.com/goatcorp/SamplePlugin.git"
  commit = "765d9bb434ac99a27e9a3f2ba0a555b55fe6269d"
  owners = ["goaaats"]
  project_path = "SamplePlugin"
  changelog = "Added Herobrine"
  ```

- 플러그인에 대한 이미지를 `images` 하위 폴더인 `stable/(plugin name)/images`에 배치합니다.
  - 이 과정은 [미래의 어느 시점에서는 간소화될 것입니다](https://github.com/goatcorp/DIPs/pull/45). 이것은 아직 [구현되지 않았습니다](https://github.com/goatcorp/DalamudPackager/issues/9). 당신이 도와줄 수 있다면, 우리는 당신의 의견을 듣고 싶습니다!
- Pull Request(이하 PR)를 하세요. GitHub 웹 에디터를 사용하고 있다면, 자동적으로 PR을 할 것입니다.

DalamudPackager도 사용하셔야 합니다. 샘플 플러그인을 통해 예시를 확인하시기 바랍니다. 도움이 필요하시면 디스코드 등으로 연락주시기 바랍니다.

## 플러그인 업데이트하기

manifest에 있는 commit hash를 편집하신 후, 해당하는 PR에서 `@currybot rebuild`라는 comment를 남겨주세요. 새 branch에서 업데이트를 해주시면 저희가 더 명확하게 검토할 수 있습니다.

## PR에서 리빌딩하기

PR에서 build를 다시 하고 싶으면, `@currybot rebuild`라는 comment를 남겨주세요. @currybot이 build를 진행해줍니다.

## Secrets

build 과정에 secrets가 필요하거나, 플러그인에 secret을 포함하려면, 디스코드를 통해 문의해주세요. 현재는 goatcorp에서 제공하는 방식의 서비스를 지원하지 않습니다.

---

플러그인을 submit할 때는, 플러그인을 이 리포지토리에 업로드할 때 부여해야 하는 권한을 자세히 설명하는 등 당사의 [이용 목적 제한 방침](<https://github.com/goatcorp/FFXIVQuickLauncher/wiki/Acceptable-Use-Policy-(Official-Plugin-Repository)>) 및 [서비스 약관](<https://github.com/goatcorp/FFXIVQuickLauncher/wiki/Terms-and-Conditions-of-Use-(XIVLauncher,-Dalamud-&-Official-Plugin-Repository)>)을 고려하십시오.

플러그인을 포기할 경우 어떻게 되는지 알기 위해서는 [플러그인 채택 정책](https://dalamud.dev/faq/adoption)을 검토하십시오. FAQ에는 다른 개발자로부터 플러그인을 넘겨받을 경우 플러그인을 제출하는 방법도 나와 있습니다.
