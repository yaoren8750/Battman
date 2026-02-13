---
title: 充電限制
---

# 充電限制

**充電限制**頁面用於配置一個實驗性的後台守護進程，使電池電量保持在指定區間內。

## 核心概念

- **限制電量**  
  設定電池充到多少百分比時停止充電。
- **恢復電量**  
  （可選）設定在低於該百分比後重新允許充電。
- **放電模式**  
  決定在放電階段是保留外部供電，還是阻斷外部供電而強制使用電池。
- **覆蓋優化電池充電**  
  在 Battman 接管充電邏輯時，可選擇是否覆蓋系統的「優化電池充電」行為。

## 工作狀態

- **允許充電** — 值守進程運行中，允許電量繼續上升至預設上限。

<p align="center" style="margin-top: -0.5em; margin-bottom: 0.5em;">
  <img src="/controls/images/Battman_badge_continue.PNG" alt="Allowing charge badge" style="max-width: 30%; height: auto; display: block; margin: 0 auto;" />
</p>

- **阻斷充電** — 值守進程運行中，在抵達上限時保持不再充電。

<p align="center" style="margin-top: -0.5em; margin-bottom: 0.5em;">
  <img src="/controls/images/Battman_badge_stop.PNG" alt="Stopping charge badge" style="max-width: 30%; height: auto; display: block; margin: 0 auto;" />
</p>

- **無外部電源** — 未檢測到適配器，無法執行限制。

<p align="center" style="margin-top: -0.5em; margin-bottom: 0.5em;">
  <img src="/controls/images/Battman_badge_noext.PNG" alt="No external power badge" style="max-width: 30%; height: auto; display: block; margin: 0 auto;" />
</p>

- **有適配器但無供電** — 連接了適配器但未輸出電流（如線材 / 協議問題或計劃任務）。

<p align="center" style="margin-top: -0.5em; margin-bottom: 0.5em;">
  <img src="/controls/images/Battman_badge_nopwr.PNG" alt="No current badge" style="max-width: 30%; height: auto; display: block; margin: 0 auto;" />
</p>

- **優化電池充電接管** — 在未覆蓋「優化電池充電」的軟模式下，系統「優化電池充電」可能忽略 Battman 的請求。需要 Battman 優先時，請先啓用「循環時覆蓋‘優化電池充電’」再啓動值守進程。

<p align="center" style="margin-top: -0.5em; margin-bottom: 0.5em;">
  <img src="/controls/images/Battman_badge_obc.PNG" alt="OBC badge" style="max-width: 30%; height: auto; display: block; margin: 0 auto;" />
</p>

## 風險提示

- 此功能仍在測試階段，可能無法在所有設備上正常工作。
- 此功能會直接寫入系統電源相關接口，並長時間運行後台代碼。
- 當前 Battman 值守進程作為常規後台進程運行，未使用系統 `LaunchDaemons`。設備重啓或執行 ldrestart 後需要手動重新啓動值守進程才能生效。
