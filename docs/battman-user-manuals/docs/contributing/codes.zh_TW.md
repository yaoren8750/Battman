---
title: 代碼貢獻
---

# 參與代碼貢獻

歡迎為 Battman 提交代碼改進。

## 開始之前

- 建議先閱讀根目錄下的 `README.md`，瞭解項目結構與構建依賴。
- 準備好合適的 iOS 工具鏈與開發 / 測試環境。

## 項目結構

Battman 使用 **Objective-C 和 C** 編寫（不使用 Swift）。主要代碼庫組織如下：

### 目錄佈局

- **`Battman/`** - 主應用程序代碼
  - **`battery_utils/`** - 電池信息與硬件交互工具
  - **`hw/`** - 硬件特定接口（例如 IOMFB）
  - **`cobjc/`** - 用於 C 兼容性的自定義 Objective-C 運行時包裝器
  - **`ObjCExt/`** - Objective-C 類別擴展
  - **`CGIconSet/`** - 自定義圖標生成代碼
  - **`brightness/`** - 亮度控制功能
  - **`scprefs/`** - 偏好設置集成
  - **`security/`** - 安全與保護代碼

### 文件命名規範

- 視圖控制器：`*ViewController.h` / `*ViewController.m`
- 自定義視圖：`*View.h` / `*View.m` 或 `*Cell.h` / `*Cell.m`
- C 頭文件：`*.h` 對應 `*.c` 或 `*.m` 實現文件
- 頭文件使用 `#pragma once` 或傳統包含保護

## 代碼風格指南

### 語言與架構

- **主要語言**：Objective-C 和 C
- **無外部依賴**：不使用 CocoaPods、Swift Packages 或第三方框架
- **不使用 Storyboards 或 XIBs**：UI 以編程方式構建
- **不使用 Xcode Assets**：資源以編程方式生成

#### 為什麼不使用 Xcode Assets 和 Storyboards？

Battman 不使用 Xcode Assets (.xcassets) 或 Storyboards/XIBs，原因如下：

- **開源工具鏈兼容性**：這些格式是 Apple 的私有商業格式，無法使用開源工具鏈構建。這會阻止項目在非 Apple 平台或使用替代構建系統上構建。

- **跨平台協作**：我們有不使用 macOS 和 Xcode 的協作者。使用這些格式會破壞可維護性，並阻止貢獻者在他們偏好的環境中工作。

- **編程式 UI 偏好**：我們始終偏好以編程方式編寫 UI，而不是在某種文件中預定義它們。這種方法提供更好的版本控制、更輕鬆的代碼審查，以及 UI 構建的更大靈活性。

#### 為什麼不使用第三方依賴？

Battman 有意避免第三方依賴（CocoaPods、Swift Packages、外部框架），原因如下：

- **控制與可維護性**：外部代碼不在我們的控制之下，我們無法響應可能影響項目的潛在未來更改、破壞性更新或棄用。

- **協作可訪問性**：並非每個協作者都能在其構建系統上拉取此類依賴。避免外部依賴可確保所有貢獻者無論其環境設置如何都能構建和處理項目，促進更好的協作。

如果您需要通常來自第三方庫的功能，請直接在代碼庫中實現它或調整必要的代碼以適合 Battman 的架構（例如，我們已完全用 Objective-C 重寫了 Swift 的 `UberSegmentedControl`）。

#### 為什麼不使用 Swift 代碼？

Battman 僅使用 Objective-C 和 C，不接受 Swift 代碼提交，原因如下：

- **穩定性與兼容性**：Swift 本身頻繁更新並帶有破壞性更改，並且總是試圖放棄對先前系統版本的支持。這會產生維護負擔和兼容性問題，與 Battman 支持 iOS 12+ 的目標相衝突。

- **工具鏈大小**：Swift 工具鏈比常規 LLVM + Clang 大得多，這對我們現有的協作者不友好，他們可能資源有限或偏好最小化工具鏈安裝。

- **不可避免的 Swift 例外**：如果您的提交無法避免使用 Swift（例如，WidgetKit 僅允許 Swift），請創建一個新的子項目來包含它。否則，所有邏輯和 UI 都應使用 Objective-C 和 C 編寫。

  如果您必須使用 Swift，您的 Swift 代碼必須滿足以下要求：
  - 您的 Swift 代碼應在 iOS 12 或更早版本上編譯和測試。
  - 您的 Swift 代碼應使用 Swift 4 或更早版本編寫。
  - 您的 Swift 代碼至少應能在 iOS 上使用 Procursus 的 Swift 工具鏈構建。

### C / Objective-C 模式

#### 單例模式

```objc
+ (instancetype)sharedPrefs {
    static BattmanPrefs *shared = nil;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        shared = [[self alloc] _init];
    });
    return shared;
}
```

#### iOS 版本檢查

```objc
if (@available(iOS 13.0, *)) {
    // iOS 13+ 代碼
} else {
    // 回退代碼
}
```
```c
if (__builtin_available(iOS 13.0, *)) {
    // iOS 13+ 代碼
} else {
    // 回退代碼
}
```

#### 調試日誌

```c
// DBGLOG 基於 NSLog()
DBGLOG(CFSTR("調試消息: %@"), value);
```
```objc
// DBGLOG 基於 NSLog()
DBGLOG(@"調試消息: %@", value);
```

#### 本地化

```c
// 對於 NSString
NSString *localized = _("String to localize");

// 對於 C 字符串
const char *localized = _C("String to localize");
```

#### 靜態初始化

```c
static dispatch_once_t onceToken;
static bool value;

dispatch_once(&onceToken, ^{
    // 一次性初始化
    value = compute_value();
});
```

### 錯誤處理

- 檢查系統調用的返回值
- 對錯誤條件使用 `os_log_error`
- 盡可能提供回退路徑
- 避免在可恢復的錯誤上崩潰

### 內存管理

- Objective-C 代碼使用 ARC
- C 代碼使用手動內存管理（malloc 的要用 free）
- 小心處理 CF 類型 - 適當使用 `CFRelease`

### 構建系統

Battman 可以使用以下方式構建：
- **Xcode**：標準 Xcode 項目
- **Makefile**：位於 `Battman/Makefile`，支持 Linux 構建

關鍵構建參數：
- `-fobjc-arc`：自動引用計數
- `-target arm64-apple-ios12.0`：iOS 12+ 部署目標
- `-DDEBUG`：調試構建
- `-DUSE_GETTEXT`：可選的 gettext 本地化支持

## 推薦工作流程

1. Fork 本倉庫，並基於此創建功能分支。
2. 使用清晰、聚焦的提交（commit）逐步完成修改。
3. 盡量將 PR 控制在易於審閱的規模，而不是一次性包含大量無關改動。
4. 在盡可能多的相關 iOS 版本 / 設備上進行測試。
5. 提交 Pull Request，並在說明中寫明：
   - 具體改動內容；
   - 測試方式與結果；
   - 對用戶可見行為的任何影響。

## 代碼審查指南

- 遵循現有的代碼風格和模式
- 保持與 iOS 12+ 的兼容性
- 盡可能在模擬器和真實設備上測試
- 使用適當的日誌記錄（調試使用 DBGLOG，生產使用 os_log）
- 確保正確的內存管理
- 為複雜邏輯或硬件特定代碼添加註釋
- 如果行為發生變化，請更新相關文檔

請勿將個人證書、密鑰等私密信息加入倉庫或構建腳本中。
