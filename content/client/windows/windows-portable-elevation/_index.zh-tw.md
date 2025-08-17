---
title: Windows 便攜式提權
weight: 49
---

Windows 便攜式程式沒有管理員權限，這可能導致以下問題：

- 當 UAC（使用者帳戶控制）視窗彈出時無法傳輸螢幕。
- 當提升的視窗（如工作管理員）彈出時，滑鼠變得無回應。

透過提升權限，RustDesk 可以在啟動或工作階段期間建立具有管理員權限的程序，使其能夠執行螢幕截圖和滑鼠操作，從而避免上述問題。

## 啟動時提權

這樣，遠端使用者在連線時無需請求提權。有兩種方法：

* 方法 1：將便攜式程式的名稱更改為包含 `-qs-`（1.2.0、1.2.1、1.2.2、1.2.3 版本以 `qs.exe` 結尾）。點擊滑鼠左鍵執行，在 UAC 視窗中點擊 `接受`。

* 方法 2：右鍵點擊並以管理員身分執行。

## 在被控端提權

被控端可以在連線時直接點擊 `接受並提權`，或在已連線時點擊 `提權`。

| 連線中 | 已連線 |
| :---: | :---: |
| ![](/docs/en/client/windows/windows-portable-elevation/images/cm_unauth.jpg) | ![](/docs/en/client/windows/windows-portable-elevation/images/cm_auth.jpg) |

## 在控制端請求提權

從動作選單選擇 `請求提權` 後，將出現以下對話方塊。如果您選擇 `要求遠端使用者進行驗證`，您將不需要輸入使用者名稱和密碼，但遠端電腦上的使用者必須具有管理員權限。如果您選擇 `傳輸管理員的使用者名稱和密碼`，遠端電腦上的使用者只需在 UAC 視窗中接受。發送請求後，請等待對方接受 UAC 視窗。確認後，將出現成功訊息。請注意，**兩種方法都需要被控端有人接受 UAC 視窗**。因此，如果對方沒有人可用，則不應在控制端請求提權。

| 選單 | 對話方塊 |
| :---: | :---: |
| ![](/docs/en/client/windows/windows-portable-elevation/images/menu.png) | ![](/docs/en/client/windows/windows-portable-elevation/images/dialog.png) |
| **等待** | **成功** |
| ![](/docs/en/client/windows/windows-portable-elevation/images/wait.png) | ![](/docs/en/client/windows/windows-portable-elevation/images/success.png) |

## 如何選擇

| 場景 | 方法 |
| :---: | :---: |
| 不需要提權 | 安裝程式 |
| 被控端沒有使用者可用 | 重新命名<br/>*或*<br/> 以管理員身分執行 |
| 被控端有使用者可用<br/>*且*<br/> 連線時立即提權<br/>*且*<br/> 點擊接受連線 | 在被控端接收連線時點擊 `接受並提權` |
| 被控端有使用者可用<br/>*且*<br/> 根據需要提權 | 在被控端的連線管理視窗點擊 `提權`<br/>*或*<br/> 在控制端請求提權 |