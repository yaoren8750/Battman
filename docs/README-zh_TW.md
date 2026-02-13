# Battman 電池管理器
被拋棄者的現代電池管理器，為它們的老設備打造。

## 屏幕截圖
<div style="width:20%; margin: auto;" align="middle">
<img src="../Screenshots/Main-zh_CN.png?raw=true" alt="Battman Main Demo" width="25%" height="25%" />
<img src="../Screenshots/Gas Gauge-zh_CN.png?raw=true" alt="Battman Gas Gauge Demo" width="25%" height="25%" />
<img src="../Screenshots/Gas Gauge-1-zh_CN.png?raw=true" alt="Battman Gas Gauge Demo 2" width="25%" height="25%" />
<img src="../Screenshots/Adapter-zh_CN.png?raw=true" alt="Battman Adapter Demo" width="25%" height="25%" />
<img src="../Screenshots/Inductive-zh_CN.png?raw=true" alt="Battman Inductive Demo" width="25%" height="25%" />
<img src="../Screenshots/Inductive-1-zh_CN.png?raw=true" alt="Battman Inductive Demo 2" width="25%" height="25%" />
<img src="../Screenshots/Serial-zh_CN.png?raw=true" alt="Battman Serial Demo" width="25%" height="25%" />
<img src="../Screenshots/Temperature-zh_CN.png?raw=true" alt="Battman Temperature Demo" width="25%" height="25%" />
<img src="../Screenshots/ChargingMgmt-zh_CN.png?raw=true" alt="Battman Charging Management Demo" width="25%" height="25%" />
<img src="../Screenshots/ChargingLimit-zh_CN.png?raw=true" alt="Battman Charging Limit Demo" width="25%" height="25%" />
<img src="../Screenshots/Thermal-zh_CN.png?raw=true" alt="Battman Thermal Demo" width="25%" height="25%" />
</div>

$${\color{grey}真正優雅的軟件講究代碼的藝術，而非外表的點綴。}$$

<br />

### 優勢
- [x] 純粹地由Objective-C與C打造
- [x] 明朗的UI，編寫自明朗的Objective-C
- [x] 不含**StoryBoards**，不含**額外的二進制文件**，不含**Xcode 資源庫**
- [x] 不含**Swift和SwiftUI**
- [x] 不含**CocoaPods**，不含**Swift 擴展包**，不含**外部代碼包依賴**，不含**第三方框架**
- [x] Xcode 編譯？不用 Xcode 編譯也可以
- [x] 編譯在 Linux（沒錯，「你需要 Mac 來做 iOS App」是 Apple 的鼓動宣傳）
- [x] 直接獲取並操作硬件最深處的原始數據
- [x] 支持 iPhone、iPad、iPod、Xcode 模擬器和 Apple 芯片的 Mac 設備（如果有人捐贈設備的話，我甚至可以支持Apple Watch和Apple TV）
- [x] 高度適配設備電池中的德州儀器電量監測計芯片
- [x] 比 IOPS 和 PowerManagement 提供的信息還要**多得多**
- [x] 檢測你的**電源適配器**、**無線充電器**、甚至你的 **MagSafe 配件**

### 只有 Battman 能做到

目前為止，其他面向iOS的電池管理工具沒能做到的事情
(截止至 9th Sun Mar 2025 UTC+0)
- [x] 完整的 **未充電原因（NotChargingReason）** 解碼，詳見 [not_charging_reason.h](../Battman/battery_utils/not_charging_reason.h)
- [x] 德州儀器 Impedance Track™ 信息的獲取
- [x] 實時充電電壓/電流的讀取
- [x] 完美運行在 Xcode Simulator （其他人在他們的軟件里用 IOPS，所以不行）
- [x] 智能充電（優化電池充電）交互
- [x] 低電量模式行為控制
- [x] 詳細的 MagSafe 配件信息
- [x] 詳細的 Lightning 線纜與配件信息
- [x] 讀取全部硬件溫度傳感器

### 前置條件

- 越獄設備，或者通過TrollStore安裝
- iOS 12+ / macOS 11+（歡迎向前移植）
- arm64（A7+ 理論上的 / M1+）
- Gettext libintl（可選，用於本地化）
- GTK+ 3（可選，用於運行在基於GTK+的桌面環境）

### 下載

前往最新 [Release](https://github.com/Torrekie/Battman/releases/latest) 以獲取預構建包。

或者，如果你希望自己構建它：

```bash
# 在 macOS，安裝 Xcode 並直接用其編譯
# 在 Linux 或者 BSD，確保一個 LLVM 跨平台編譯工具鏈和 iPhoneOS.sdk 已經準備好，並且按需修改 Battman/Makefile
# 在 iOS，當你使用 Torrekie/Comdartiwerk 作為基礎套件時
apt install git odcctools bash clang make sed grep ld64 ldid libintl-dev iphoneos.sdk
git clone https://github.com/Torrekie/Battman
# 如果目標 iOS 12 或更早，下載 SF-Pro-Display-Regular.otf，然後放在 Battman/
wget <https://LINK/OF/SF-Pro-Display-Regular.otf> -O Battman/SF-Pro-Display-Regular.otf
cd Battman
make -C Battman all
# 生成的 Battman.ipa 將位於 $(CWD)/Battman/build/Battman.ipa
```

### 已知問題
- Battman 在 **A7 至 A10** 的設備運行時並未真正與硬件交互，因為沒有AppleSMC，作為替代這些設備使用我們無法測試的AppleHPM。

### 已測試設備
- iPhone 12 系列 (D52)
- iPad Pro 2021 第三代 (J51)
- iPhone XR
- iPad Air 2

如果 Battman 未能在你的設備正常工作，請提交[疑述](../../../issues/new)。

### 即將實現
- [ ] AppKit/Cocoa UI for macOS
- [ ] GTK+/X11 UI for iOS/macOS
- [ ] 自動識別電量計芯片
- [ ] 可選的數據採集（用於解碼當前未知的參數）
- [x] 高級功能（Apple 系統管理控制器/Apple 電源管理 操作界面）
- [x] 溫度控制
- [ ] 作為命令行程序運行
- [ ] 作為守護進程運行
- [x] 充電限制
- [x] 無線/MagSafe 適配
- [ ] App 能耗限制
- [ ] Jetsam 控制
- [ ] 風扇控制
- [ ] 藍牙配件（AirPods等）

### 許可證

自 27th Sat Sep 2025 UTC+0 起，Battman 由 MIT 轉變為[非自由協議](../LICENSE/LICENSE.md)，你不會因為我想以此維持生計而怪罪於我，對吧？

## 免責聲明

**請勿用於生產環境，無任何保障，風險自負。**
