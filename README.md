# Spotify_Clone

🌱 Spotify 앱 따라 만들기

Supported By [iOS Academy - Building Spotify App Course](https://www.youtube.com/watch?v=im5n5gpTHTM&list=PL5PR3UyfTWve9ZC7Yws0x6EGjBO2FGr0o&index=2)

💁🏻 · 다시 잘 사용할 수 있게, 잘 정리해놓자

<br />
<br />

## Get Started

- 아래의 과정들은 프로젝트 시작 전 기본적으로 설정해야하는 것들에 속한다.
  - 프로젝트마다 형식과 방법은 달라질 수 있겠지만, 자주 사용하는 것들이므로 잘 숙지하자
  - 아키텍처(폴더링), 메인스토리보드없이 시작, 탭바컨트롤러 세팅
- .gitignore 설정 - `swift`, `macos`, `xcode`, `cocoapods`

### 01. 아키텍처 세팅

```Markdown
🗂 Resources

    - Assets/

    - AppDelegate

    - SceneDelegate

🗂 Managers : 데이터 매니저

    - AuthManager.swift

    - APICaller.swift

    - HapticsManager.swift

🗂 ViewModels

🗂 Models : 데이터 모델 저장 (ex. Playlist.swift, AudioTrack.swift ...)

🗂 Views

    - LauchScreen.storyboard

🗂 Controllers

    - Core 🗂 : 탭바 컨트롤러를 중심으로 핵심이 되는 ViewControllers

      - TabBarController

      - HomeViewController

      - ...

    - Other 🗂 : 나머지 ViewControllers

```

### 02. 메인스토리보드없이 시작

```
1. Targets - General - Deployment Info - Main Interface - Main 제거

2. info.plist
    Application Scene Manifest
    ㄴ Scene Configuration
       ㄴ Application Session Role
          ㄴ Application Session Role
              ㄴ Item 0 - Storyboard Name: Main 제거

3. SceneDelegate에서 RootViewController 지정
```

▶︎ SceneDelegate 설정

```swift
guard let windowScene = (scene as? UIWindowScene) else { return }

let window = UIWindow(windowScene: windowScene) 🚨

if AuthManager.shared.isSignedIn {
    window.rootViewController = TabBarViewController()
}
else {
    let navVC = UINavigationController(rootViewController: WelcomeViewController()) 🚨
    navVC.navigationBar.prefersLargeTitles = true
    navVC.viewControllers.first?.navigationItem.largeTitleDisplayMode = .always
    window.rootViewController = navVC 🚨
}

window.makeKeyAndVisible() 🚨
self.window = window 🚨
```

### 03. 탭바컨트롤러 세팅 (Base Controller)

```swift
override func viewDidLoad() {
    super.viewDidLoad()

    let vc1 = HomeViewController()
    let vc2 = SearchViewController()
    let vc3 = LibraryViewController()

    vc1.title = "Browse"
    vc2.title = "Search"
    vc3.title = "Library"

    vc1.navigationItem.largeTitleDisplayMode = .always
    vc2.navigationItem.largeTitleDisplayMode = .always
    vc3.navigationItem.largeTitleDisplayMode = .always

    let nav1 = UINavigationController(rootViewController: vc1)
    let nav2 = UINavigationController(rootViewController: vc2)
    let nav3 = UINavigationController(rootViewController: vc3)

    nav1.tabBarItem = UITabBarItem(title: "Home", image: UIImage(systemName: "house"), tag: 1)
    nav2.tabBarItem = UITabBarItem(title: "Search", image: UIImage(systemName: "magnifyingglass"), tag: 1)
    nav3.tabBarItem = UITabBarItem(title: "Library", image: UIImage(systemName: "music.note.list"), tag: 1)

    nav1.navigationBar.prefersLargeTitles = true
    nav2.navigationBar.prefersLargeTitles = true
    nav3.navigationBar.prefersLargeTitles = true

    setViewControllers([nav1, nav2, nav3], animated: false)
}
```

### 04. SF Symbols 설치

▶︎ macOS Catalina 이상이 되면 SF Symbols app을 다운로드받을 수 있음!
