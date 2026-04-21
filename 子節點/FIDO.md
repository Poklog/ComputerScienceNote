**FIDO (Fast Identity Online)** 是一種具備高安全性、便利性與可擴展性的身分鑑別機制。其核心設計理念是採用**公開金鑰密碼學**技術，用來取代傳統需要傳輸或儲存用戶帳號密碼的作法，從而有效解決密碼被盜、暴力破解、猜測攻擊或用戶遺忘密碼等問題。


### 核心安全架構

- **非對稱加密模型**：FIDO 採用公開金鑰架構。身分鑑別伺服器（Relying Party Server）端僅保存相對應的**公鑰**，而**私鑰**則安全地保存在使用者裝置端的驗證器（Authenticator）中。
- **隱私保護**：由於伺服器端不儲存使用者的密碼或個人資料，且驗證過程中不會上傳個人資訊，能有效防止大規模的資料外洩與釣魚攻擊。

### 運作流程

FIDO 的運作主要分為註冊與登入兩個階段：

- 註冊流程：
	1. **發送註冊請求**：用戶透過瀏覽器（Browser）向身分鑑別伺服器（RP Server）發送註冊請求及帳號資訊
	2. **生成建立選項**：RP Server 生成並傳送 `PublicKeyCredentialCreationOptions` 給瀏覽器，內容包含挑戰值（Challenge）、RP 資訊及用戶資訊
	3. **生成 clientData**：瀏覽器生成 `clientDataHash` 等資料，並傳送至身分驗證器（Authenticator）
	4. **驗證身分與金鑰生成**：身分驗證器（如手機生物辨識、NFC 或 USB 金鑰）驗證使用者身分後，**生成一組金鑰對（Key Pair）**，將**私鑰（Secret Key）儲存於驗證器內**，並生成簽章（Attestation）
	5. **回傳認證物件**：驗證器將 `attestationObject`（包含公鑰與憑證 ID）及 `clientData` 回傳至瀏覽器
	6. **解析並傳送憑證**：瀏覽器解析後，將 `PublicKeyCredential` 資訊傳送給 RP Server
	7. **伺服器端驗證與儲存**：RP Server 驗證收到的憑證，並儲存公鑰（PublicKey）與 `CredentialID`，完成註冊

- 登入驗證流程：
	1. **發送驗證請求**：用戶向 RP Server 發起登入請求
	2. **回傳請求選項**：RP Server 回傳 `PublicKeyCredentialRequestOptions`（包含挑戰值、RP ID、憑證列表等）
	3. **驗證來源與生成 Hash**：身分驗證器驗證來源（Origin）合法性，並生成 `clientDataHash`
	4. **身分驗證與簽章生成**：驗證器再次驗證使用者身分（如指紋或 PIN 碼），驗證成功後**利用私鑰對資料進行簽署**，生成簽章（Assertion）
	5. **回傳簽章資訊**：驗證器將簽章後的資訊（如 `authenticatorData`、`signature`）回傳給瀏覽器
	6. **解析並傳送至伺服器**：瀏覽器解析 `PublicKeyCredential` 後傳送至 RP Server
	7. **伺服器端解析與核對**：RP Server 解析收到的資訊，並利用先前儲存的**公鑰來驗證簽章**，確認身分後允許登入


### FIDO 機制的優點

- **無密碼體驗**：提供完善的無密碼登入標準，提升使用者便利性。
- **支持多因子鑑別 (MFA)**：FIDO 支援多因子驗證，進一步強化系統安全性。
- **跨平台一致性**：使用者可以在不同的設備、網站或應用程序上，使用同一個身分鑑別方法（如指紋、臉部辨識或實體金鑰）。
- **降低管理負擔**：支援 **Multi-server** 架構，能夠降低金鑰管理的複雜度與負擔。
