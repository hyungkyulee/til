# Setup GitHub Copilot for Xcode

## 1. setup and configuration
```
brew install --cask github-copilot-for-xcode
```

## 2. Core permissions

- run the installed "github copilot for xcode" app
<img width="623" height="152" alt="image" src="https://github.com/user-attachments/assets/e008ac38-ceef-435e-a7b5-652a11ca09aa" />

- enable the permissions
  - accessibility
    <img width="449" height="172" alt="image" src="https://github.com/user-attachments/assets/50d8d574-d69b-4290-af04-1a89161d4bc1" />
    <img width="517" height="104" alt="image" src="https://github.com/user-attachments/assets/0fb28a32-a9e3-4624-9c45-0ab4a78c6802" />

  - Xcode Source Editor Extension
    system settings > general > app background activity > enable 'GitHub Copilot for Xcode'
    <img width="708" height="357" alt="image" src="https://github.com/user-attachments/assets/0207a199-a7e8-4098-8e20-97ab5c344d31" />


3. Sync with GitHub account

- ensure the current focused application is Xcode
- run the installed "github copilot for xcode" app again to sync the copilot with xcode

5. How to use
- 코드 자동 완성: 코드를 타이핑할 때 인라인(고스트 텍스트) 형태로 코드가 추천됩니다. 탭(Tab) 키를 눌러 적용할 수 있습니다. ⁠Xcode extension for GitHub Copilot
- 제안된 전체 코드 보기: Option 키를 길게 누르면 제안 전체를 볼 수 있으며, Option + Tab을 눌러 제안된 전체 코드를 수락할 수 있습니다.
- Xcode extension for GitHub Copilot추천 충돌 방지: Xcode 자체의 자동 완성 기능(Predictive Code Completion)과 충돌할 수 있으므로, Xcode 설정(Settings -> Text Editing -> Editing)에서 Predictive code completion을 끄는 것을 권장합니다. Xcode extension for GitHub Copilot
<img width="690" height="326" alt="image" src="https://github.com/user-attachments/assets/ac3d8514-aca8-46f8-bd80-530a2958c58c" />

