# 舞萌手台 Pico 電路接線整理

資料來源:手繪接線圖「舞萌手台」(Pico 腳位圖上直接畫線)、`firmware/src/`(`board_defs.h`、`button.c`、`touch.c`、`rgb.c`、`config.c`)、`README.md`。

本文以手繪接線圖為準:所有模組都是外接,**不使用 mai_pico 的 IO PCB**。沒用到的 mai_pico 原版設計集中放在第 8 節,並標註「本專案未使用」。韌體預設值(按鈕對應、I2C 位址、RGB 順序)仍沿用 mai_pico 的設定。

`firmware/modules/aic_pico` 自己的 `board_defs.h`(i2c0、GPIO20/21、GPIO12)屬於另一個產品,這裡沒有使用。

## 1. 整體架構

```
USB ──5V──► Pico VBUS(pin 40) ─┬─► 8 顆按鈕的光學感測 LED(各經 220Ω)
                               └─(旁邊接一些電容到 GND)

外部 DC 5V ──► WS2812 燈鏈 VCC       (GND 與 Pico 共地)

Pico 3V3(OUT)(pin 36) ─┬─► 4 顆 MPR121 VCC
                       └─► 8 顆按鈕的上拉電阻(各 5.1kΩ)

Pico ─GP6/GP7 (I2C1)─┬─ MPR121 ×4(0x5A / 0x5B / 0x5C / 0x5D)─► ITO
                     └─ PN532 0x24(AIME,選配,圖上沒畫)
Pico ─GP0~5, 8, 9 ─► 8 顆主按鈕
Pico ─GP10, 11, 12, 14 ─► Navigate / Service / Test / Coin 四顆功能鍵
Pico ─GP13 ─► WS2812 資料線
```

## 2. Pico 腳位總表

| GPIO | 實體腳 | 用途 | 接到 |
|---|---|---|---|
| GP0 / GP1 / GP2 / GP3 | 1 / 2 / 4 / 5 | 按鈕訊號 | 主按鈕 ×4(圖上用紅框標出) |
| GP4 / GP5 | 6 / 7 | 按鈕訊號 | 主按鈕 ×2 |
| **GP6** | 9 | I2C1 SDA | 4 顆 MPR121(藍線) |
| **GP7** | 10 | I2C1 SCL | 4 顆 MPR121(藍線) |
| GP8 / GP9 | 11 / 12 | 按鈕訊號 | 主按鈕 ×2 |
| GP10 | 14 | Navigate | 功能鍵 |
| GP11 | 15 | Service | 功能鍵 |
| GP12 | 16 | Test | 功能鍵 |
| **GP13** | 17 | WS2812 資料線 | 燈鏈 DIN(直連,沒有電平轉換) |
| GP14 | 19 | Coin | 功能鍵 |
| 3V3(OUT) | 36 | 3.3V | MPR121 VCC、按鈕上拉電阻 |
| VBUS | 40 | 5V(來自 USB) | 按鈕 LED(經 220Ω) |
| GND | 18、38 | 地 | MPR121、按鈕、功能鍵、外部 DC |

- 圖上主按鈕 8 顆各自拉一條綠線回 GPIO;功能鍵的綠線在圖上交錯,GP10/11/12/14 的對應是依韌體預設推定,不是從線路逐條讀出。
- 功能鍵 4 顆共用一條 GND,接到 Pico pin 18。
- VBUS(pin 40)與 VSYS(pin 39)之間畫了電容符號,旁註「一些…電容」(字跡難辨,推測是濾波/穩壓電容)。
- 外部 DC 5V 的 GND 必須和 Pico GND 接在一起。

## 3. 按鈕

### 主按鈕 ×8(光學感測)

每顆按鈕都是同一組接法(圖右側「Buttons ×8」):

| 腳位 | 接法 |
|---|---|
| 電源 | 經 **220Ω** 接 5V(VBUS),給光學感測 LED |
| 訊號 | 接 GPIO,並經 **5.1kΩ** 上拉到 3.3V |
| GND | 共地 |

### 功能鍵 ×4

Navigate、Coin、Service、Test 各一顆,一腳接 GPIO,另一腳共地(Pico pin 18)。

### 韌體預設的按鈕與 GPIO 對應(`BUTTON_DEF`)

| 韌體按鈕 | B1 | B2 | B3 | B4 | B5 | B6 | B7 | B8 | Test | Service | Navigate | Coin |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| GPIO | 1 | 0 | 4 | 5 | 8 | 9 | 3 | 2 | 12 | 11 | 10 | 14 |

- 預設低電位有效,韌體開內部上拉,按下時把訊號接到 GND。
- 韌體只允許這 12 個 GPIO:0、1、2、3、4、5、8、9、10、11、12、14。
- 可用 `gpio` 指令或網頁設定器重新對應。
- 長按 Coin 2 秒會重新初始化觸控。
- 圖上沒標哪顆實體按鈕接哪個 GPIO,上表是韌體預設;實際接線不同就用 `gpio` 指令改。

## 4. 觸控:I2C 位址與接線

4 顆 MPR121 全是外接模組,SDA 接 GP6、SCL 接 GP7,VCC 接 3V3,GND 共地,並聯在同一條匯流排上。

| 模組 | 位址 | ADDR 接法 |
|---|---|---|
| #0 | 0x5A | GND |
| #1 | 0x5B | 3V3 |
| #2 | 0x5C | SDA |
| #3 | 0x5D | SCL |

- 圖上每顆模組的 ADD 腳都有拉線,但線色和走向看不出各自接到哪裡,位址以上表為準,接好後請用 I2C 掃描確認。
- 所有模組的 IRQ 都沒接,韌體用輪詢。
- README 要求每顆模組先割斷 ADDR 到 GND 的小跳線,否則位址會被鎖在 0x5A。
- 第 4 顆預設不對應任何 zone,要自己用 `touch 3 <電極> <zone>` 設定。
- 模組的電極端接 ITO 玻璃(圖上標 ITOs),韌體預設 zone 如下:

| 模組 | 預設 zone(依電極 ELE0 起順序) |
|---|---|
| #0(0x5A) | E3 A2 B2 D2 E2 A1 B1 D1 E1 C2 A8 B8 |
| #1(0x5B) | D8 E8 A7 B7 D7 E7 A6 B6 D6 E6 A5 B5 |
| #2(0x5C) | D5 E5 C1 A4 B4 D4 E4 A3 B3 D3(ELE10、ELE11 為 XX) |
| #3(0x5D) | 無預設 |

## 5. RGB 燈鏈(GP13,GRB,800kHz)

```
GP13 → 按鈕 4 → 3 → 2 → 1 → 8 → 7 → 6 → 5 → 機櫃燈 ×3 → banner → AIME 燈 16 顆
```

- 順序由 `RGB_BUTTON_MAP` 決定。
- 預設每顆按鈕 4 顆串聯、機櫃每節點 1 顆、banner 0 顆。這些數字用 `rgb` 指令改。
- AIME 固定 16 顆,不能改。
- **供電**:圖上燈鏈(W2812)的 5V 來自外部 DC,不從 Pico 取電;資料線接 GP13,GND 與 Pico 共地。
- 燈鏈資料線是 3.3V 邏輯直連 5V 的 WS2812,多數情況可用;若燈珠不穩,可在 GP13 加電平轉換或串一顆電阻。

## 6. 其他連接

- **PN532(AIME)**:圖上沒有。SDA 接 GP6、SCL 接 GP7,位址 0x24,撥碼切到 I2C 模式。VCC 接 3V3 是依 AIC Pico 的接線圖推得,mai_pico 的 README 沒明講。
- **USB**:Pico 用自己的 micro-USB 供電與通訊,5V 由 VBUS 再供給按鈕 LED。

## 7. 需要核對的地方

1. **MPR121 的 ADD 接法**:圖上的 ADD 線看不清楚,請實際量測或掃描 I2C,確認 4 顆位址各不相同。
2. **功能鍵腳位**:GP10/11/12/14 與 Navigate/Service/Test/Coin 的對應是依韌體預設推定。
3. **VBUS 電流**:8 顆按鈕 LED 都由 USB 5V 供電,每顆約數 mA 到十幾 mA(取決於 LED 與 220Ω),通常夠用;燈鏈已改由外部 DC 供電,不會經過 Pico。
4. **每顆按鈕的燈數**:韌體預設 `per_button=4`,實際燈珠數量不同就用 `rgb` 指令修正。
5. **`rgb.c` 用的巨集名稱與 `board_defs.h` 不同**。`rgb.c` 判斷的是 `BUTTON_RGB_ORDER`,但 `board_defs.h` 定義的是 `RGB_ORDER`。兩者都沒定義時,預處理器會當成 0 == 0,剛好落在 GRB 分支。現在顏色順序正確,但改 `RGB_ORDER` 不會有任何作用。
6. 本文是依手繪圖解讀,圖上的導線密集處(Pico 左側與底部)可能有讀錯,焊接前請再對照一次。

## 8. mai_pico 原版設計(本專案未使用,僅供對照)

以下是 mai_pico 原版 PCB(`PCB/IO/mai_io.kicad_pcb`、`PCB/Button/mai_button.kicad_pcb`)的設計,本專案沒有用到,接線不要照這一節做。

### 電源與架構

```
USB Type-C ──5V──► VSYS ─┬─► WS2812 鏈(RGB0)
 (D+/D- → Pico TP2/TP3)  └─► 按鈕光學感測 LED(經 220Ω)

Pico ─GP6/GP7 (I2C1, 100kHz)─┬─ MPR121 #0 0x5A ─┐
                             ├─ MPR121 #1 0x5B ─┼─► J1 34 針 ─► 斑馬紙 ─► ITO 玻璃
                             ├─ MPR121 #2 0x5C ─┘
                             └─ MPR121 #3 0x5D(外接,PCB 上沒有)
```

- 5V 由 Type-C 直接進 VSYS,Pico 的 VBUS(40 腳)沒接。**本專案改用 VBUS 供按鈕 LED、外部 DC 供燈鏈。**
- Type-C 的 D−、D+ 接 Pico 底部測試點 TP2、TP3,CC1、CC2 各經 5.1kΩ(R20、R21)到 GND,SBU 沒接。**本專案不用。**
- WS2812 資料線 GP13 接 IO PCB 的 RGB0 pin2(pin1=+5V、pin3=GND)。**本專案直接從 GP13 拉線。**

### IO PCB 按鈕連接器(JST-EH)

| 連接器 | pin1 | pin2 | pin3 |
|---|---|---|---|
| BT1~BT8(3 針) | 經 220Ω(R1~R8)到 +5V,給光學感測 LED | GPIO,10kΩ(R9~R16)上拉到 3V3 | GND |
| AUX1 / AUX2(2 針) | GPIO | GND | — |

- 原版上拉電阻是 10kΩ(README 寫 5.1kΩ);**本專案按圖用 5.1kΩ。**
- 原版 GP0~5、8、9 接 BT 連接器,GP10、GP11 接 AUX1、AUX2,GP12(Test)、GP14(Coin)在 PCB 上沒有走線。**本專案四顆功能鍵直接接 GPIO。**

| 韌體按鈕 | B1 | B2 | B3 | B4 | B5 | B6 | B7 | B8 | Test | Service | Navigate | Coin |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 原版 PCB 接頭 | BT2 | BT1 | BT5 | BT6 | BT7 | BT8 | BT4 | BT3 | 無 | AUX2 | AUX1 | 無 |

### 單顆按鈕板(`mai_button`)

| 零件 | 連接 |
|---|---|
| J1 RGB In | 1=+5V、2=DIN、3=GND |
| J3 RGB Out | 1=+5V、2=DOUT(來自 D1)、3=GND |
| J2、J4 Btn | 兩個焊點同一網路,接開關 SW1 pin2 |
| SW1 | pin1=GND |
| D1、D2 | WS2812B-2835,DIN 並聯,各自 VDD 接 5V |
| C1 | 0.1µF 在 5V 與 GND 之間 |

注意:這塊板的網表是 D1 與 D2 並聯、D2 輸出懸空,只有 1 顆串聯,與 README 和韌體預設 `per_button=4` 不一致。

### 內建 MPR121 與 J1 排針

IO PCB 內建 U5(0x5A,ADDR=GND)、U6(0x5B,ADDR=3V3)、U7(0x5C,ADDR=SDA),第 4 顆(0x5D,ADDR=SCL)外接。本專案 4 顆都是外接模組。

| J1 腳位 | 模組 | 預設 zone |
|---|---|---|
| 1–12 | U5(0x5A) | E3 A2 B2 D2 E2 A1 B1 D1 E1 C2 A8 B8 |
| 13–24 | U6(0x5B) | D8 E8 A7 B7 D7 E7 A6 B6 D6 E6 A5 B5 |
| 25–34 | U7(0x5C) ELE0–9 | D5 E5 C1 A4 B4 D4 E4 A3 B3 D3 |
| 未接出 | U7 ELE10、ELE11 | XX XX |

PCB 上的網路名 `/A1…/C12` 是「第幾顆晶片的第幾個電極」,不是遊戲的 zone 名稱。例如 J1 pin1 的網路叫 `/A1`,但預設 zone 是 E3。J1 經斑馬紙接 ITO 玻璃,本專案沒有 J1。

### 原版 PCB 的其他備註

- 本節只讀了 KiCad 原始檔,`Production/PCB/*.zip` 的 Gerber 沒有解開比對。
