# 免責聲明與授權 / Disclaimer and License

[繁體中文](#繁體中文) | [English](#english)

---

# 繁體中文

## 免責聲明
開始製作前請先閱讀。只要你參考或使用本專案中的任何內容(包含README.md、README_EN.md、WIRING.md、設計圖、材料表與韌體設定)，即表示你了解並同意以下事項:

1. **非官方，也與遊戲檔案無關**: 本專案是個人 DIY 紀錄，與SEGA及任何官方營運方無關，未經其認可；maimai、舞萌、Aime等相關名稱與商標屬於其權利人。本專案不提供、也不協助取得任何遊戲檔案或破解，只涉及控制器硬體與韌體。請自行確保你的行為符合所在地的法律，以及遊戲與機台相關的授權與服務條款。
2. **僅為個人紀錄**: 本專案記錄的是作者個人的製作經驗，不是專業的工程、電機或木工指引，也不保證內容完整、正確，或適用於你的情況。
3. **風險自負，作者不負責製作流程**: 依本專案製作屬於你的自願行為。作者不負責製作流程及其產生的任何後果，包含但不限於人身傷害、觸電、火災、玻璃破裂、機台傾倒或掉落、電視、電腦、Pico等設備損壞、財物損失、材料與金錢的浪費、資料遺失。所有內容皆依「現狀」提供，不附帶任何形式的保證，作者不承擔任何直接或間接責任。
4. **安全提醒(並非完整的安全指引)**:
   - 電視與電源接的是市電，請勿自行拆開或改裝電源板
   - 通電前請先用電表檢查有沒有短路
   - 43吋電視加上支架很重，請確實固定並防止傾倒，搬運與組裝需要兩個人
   - 鋼化玻璃破裂時很危險，邊緣也很鋒利，請戴手套並小心搬運
   - 使用電鑽、絲攻、電烙鐵時請做好防護，並讓小孩與寵物遠離
5. **內容可能有誤**: 電路圖、WIRING.md(由手繪圖整理而成，可能讀錯)、尺寸、價格與型號都可能有誤或已過時，請自行查證，並以實際零件與原廠資料為準。
6. **AI生成內容**: 韌體fork與部分文字是在AI協助下產生，沒有經過嚴謹驗證，使用前請自行檢查與測試。
7. **商品與店家**: 文中提到的商品、店家與平台僅是個人購買紀錄，不是廣告或背書，作者也沒有收取任何贊助。價格、品質與供貨狀況隨時可能改變。
8. **不提供支援義務**: 歡迎開issue討論，但作者沒有義務回覆、修正錯誤或持續維護。與遊戲相關的問題不會公開回答。
9. **第三方內容**: 文中引用的圖片、截圖、文件與連結，其著作權屬於原權利人，僅用於說明。若權利人要求移除，請透過issue或IG聯絡，我會盡快處理。

## 授權
除下方「不在授權範圍內」所列的內容外，本專案中由作者原創的文字、照片與圖片，以 **[創用CC 姓名標示-非商業性-相同方式分享 4.0 國際 (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh_TW)** 授權，完整條款見 [LICENSE](LICENSE)。

Copyright (c) 2026 samuelhsieh0829

簡單來說，你可以:
- **分享**: 以任何媒介或格式重製、散布本專案內容
- **修改**: 重混、轉換或依本專案內容創作

但必須遵守:
- **姓名標示**: 註明作者(samuelhsieh0829)、附上授權連結，並標示是否有修改
- **非商業性**: 不得用於商業目的(例如販售本指南、販售依本指南製作的成品來營利)
- **相同方式分享**: 如果你修改或衍生出新內容，必須以相同的授權散布

以上只是摘要，若與 [LICENSE](LICENSE) 條文有出入，以條文為準。

### 不在授權範圍內
下列內容的著作權屬於原權利人，**不適用**本專案的授權，請依其各自的授權或規範使用:
- 引用自他處的圖片與截圖，包含遊戲畫面(© SEGA)、README中標示「擷取自」的圖片(例如 `sensor_theme.webp`、`sensor_placement.jpe`、`rc.png`、`wtfisthis.jpe`、`backbone.png`、`ito.png`)，以及周邊商品、其他作品的畫面
- 上游專案的檔案與設計，例如 [Syndric/maimai-controller-fullsize](https://github.com/Syndric/maimai-controller-fullsize)(CAD、DXF)、[whowechina/mai_pico](https://github.com/whowechina/mai_pico)，這兩個專案的 LICENSE.txt 皆為 [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/)(姓名標示-非商業性)，這部分內容仍受該授權約束，請依其條款使用並標示出處
- 韌體fork [samuelhsieh0829/mai_pico](https://github.com/samuelhsieh0829/mai_pico) 有它自己的授權，不受本專案授權影響
- maimai、舞萌、Aime、SEGA 等名稱與商標，屬於其權利人

---

# English

## Disclaimer
Please read this before you start building. By following or reusing anything in this repository (including README.md, README_EN.md, WIRING.md, the design drawings, the parts list and the firmware settings), you understand and agree to the following:

1. **Unofficial, and unrelated to game files.** This is a personal DIY record. It is not affiliated with SEGA or any official operator and has not been endorsed by them. maimai, 舞萌, Aime and related names and trademarks belong to their respective owners. This project does not provide, and does not help you obtain, any game files or cracks; it only covers the controller hardware and firmware. You are responsible for following the laws, licenses and terms of service that apply to you and to the game and cabinet.
2. **Personal record only.** This repository records one person's build experience. It is not a professional engineering, electrical or woodworking guide, and it does not guarantee that the content is complete, correct or suitable for your situation.
3. **Use at your own risk; the author is not responsible for the build process.** Building from this project is your own voluntary choice. The author takes no responsibility for the build process or anything that results from it, including but not limited to personal injury, electric shock, fire, broken glass, the controller tipping over or falling, damage to TVs, computers, Pico boards or other equipment, loss of property, wasted materials and money, or data loss. Everything is provided "as is", without warranty of any kind, and the author accepts no direct or indirect liability.
4. **Safety reminders (not a complete safety guide).**
   - The TV and power supply run on mains voltage. Do not open or modify a power board yourself.
   - Check for short circuits with a multimeter before you power anything on.
   - A 43-inch TV plus the frame is heavy. Fix it firmly, prevent it from tipping over, and get two people for moving and assembly.
   - Tempered glass can shatter and its edges are sharp. Wear gloves and handle it with care.
   - Wear protection when using drills, taps and soldering irons. Keep children and pets away.
5. **The content may be wrong.** Circuit diagrams, WIRING.md (which was put together from a hand-drawn diagram and may be misread), dimensions, prices and part numbers may be wrong or out of date. Verify everything yourself and rely on the actual parts and manufacturer documentation.
6. **AI-generated content.** The firmware fork and parts of the text were produced with AI assistance and have not been rigorously verified. Review and test them yourself before relying on them.
7. **Products and shops.** Any product, shop or platform mentioned is only a record of what the author bought. It is not an advertisement or endorsement, and the author received no sponsorship. Prices, quality and availability may change at any time.
8. **No support obligation.** You are welcome to open an issue, but the author has no obligation to reply, fix errors or keep the repository maintained. Game-related questions will not be answered publicly.
9. **Third-party content.** Images, screenshots, documents and links taken from other sources belong to their original rights holders and are used for illustration only. If a rights holder asks for something to be removed, contact me through an issue or IG and I will handle it promptly.

## License
Except for the content listed under "Not covered by this license" below, the original text, photos and drawings in this repository are licensed under **[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/)**. The full terms are in [LICENSE](LICENSE).

Copyright (c) 2026 samuelhsieh0829

In short, you may:
- **Share**: copy and redistribute the material in any medium or format
- **Adapt**: remix, transform and build upon the material

Under these terms:
- **Attribution**: credit the author (samuelhsieh0829), link to the license, and indicate if changes were made
- **NonCommercial**: you may not use the material for commercial purposes (for example, selling this guide, or selling controllers built from it for profit)
- **ShareAlike**: if you remix, transform or build upon the material, you must distribute your contributions under the same license

This is only a summary. If it differs from the text of [LICENSE](LICENSE), the license text prevails.

### Not covered by this license
The following belong to their original rights holders and are **not** covered by this repository's license. Use them under their own licenses or terms:
- Images and screenshots taken from elsewhere, including game screens (© SEGA), images marked "captured from" in the README (for example `sensor_theme.webp`, `sensor_placement.jpe`, `rc.png`, `wtfisthis.jpe`, `backbone.png`, `ito.png`), and screens or merchandise from other works
- Files and designs from upstream projects, such as [Syndric/maimai-controller-fullsize](https://github.com/Syndric/maimai-controller-fullsize) (CAD, DXF) and [whowechina/mai_pico](https://github.com/whowechina/mai_pico). Both projects' LICENSE.txt files are [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/) (Attribution-NonCommercial). That material stays under that license, so use it under those terms and credit the source
- The firmware fork [samuelhsieh0829/mai_pico](https://github.com/samuelhsieh0829/mai_pico) has its own license, which this repository's license does not change
- Names and trademarks such as maimai, 舞萌, Aime and SEGA belong to their respective owners
