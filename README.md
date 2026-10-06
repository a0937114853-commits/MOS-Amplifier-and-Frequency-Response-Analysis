# MOS Amplifier and Frequency Response Analysis - README

本專案為 MOS 放大器與頻率響應分析之作業與模擬整理，由 Chao-Yang Zhan 撰寫，基於 $0.5\ \mu\text{m}$ CMOS 製程（SPICE Level-1 模型）進行完整的電路設計、直流偏壓、頻率響應、動態範圍及寄生效應分析。內容結合文字說明與模擬圖表，提供直觀且嚴謹的效能評估。

---

## 📋 系統設計規格 (Design Specifications)
* **供應電壓 ($V_{DD}$)**：$3\text{ V}$
* **訊號源內阻 ($R_{sig}$)**：$3\text{ k}\Omega$
* **負載電阻 ($R_L$)**：$10\text{ k}\Omega$
* **耦合與旁路電容 ($C_{CI}, C_{CO}, C_S$)**：$5\ \mu\text{F}$
* **設計目標**：電壓增益大小 $|v_o/v_{sig}| \ge 30$ 且總功率消耗 $< 1\text{ mW}$

---

## 1. 偏壓電流與 W/L 比例設計
* **製程參數**：$\mu_n = 460\text{ cm}^2/\text{V}\cdot\text{s}$，$t_{ox} = 9.5\text{ nm}$，$C_{ox} \approx 3.63\times 10^{-3}\text{ F/m}^2$，$k_n' \approx 167\ \mu\text{A/V}^2$。
* **工作點與功率**：設定 $I_D = 100\ \mu\text{A}$，分壓網路電流 $1.5\ \mu\text{A}$，總電流 $201.5\ \mu\text{A}$，總功率消耗 $P = 0.6045\text{ mW}$（符合 $< 1\text{ mW}$ 標準）。
* **元件尺寸與偏壓**：晶體 $M_1$ 尺寸 $W_1 = 134.5\ \mu\text{m}$ ($L_1 = 0.5\ \mu\text{m}$)；源極隨耦器 $M_2$ 尺寸 $W_2 = 15\ \mu\text{m}$ ($L_2 = 0.5\ \mu\text{m}$)。設定 $R_D = 10\text{ k}\Omega$、$R_S = 10\text{ k}\Omega$ 以滿足 $V_{DS1} = 1\text{ V}$。

| 電路原理圖與配置 | DC 工作點模擬結果 |
| :---: | :---: |
| ![Schematic](figures/problem_a_schematic.png)<br>*圖 1：MOS 放大器完整電路原理圖* | ![Op Point](figures/problem_a_op.png)<br>*圖 2：DC Operating Point 節點電壓與電流* |

---

## 2. 頻率響應分析
* **中頻增益**：AC 掃描（$1\text{ Hz}$ 至 $1\text{ GHz}$）顯示中頻增益約為 $30.17\text{ dB}$，符合 $\ge 30$ 之設計規範。
* **頻寬與功耗**：低頻截止頻率 $f_L \approx 115.5\text{ Hz}$，高頻截止頻率 $f_H \approx 23.27\text{ MHz}$。實際總功率消耗 $P_{actual} \approx 0.6083\text{ mW}$。

| 增益頻率響應曲線 | 相位頻率響應與 3-dB 標註 |
| :---: | :---: |
| ![AC Response](figures/problem_b_ac_response.png)<br>*圖 3：AC 掃描之電壓增益對數頻率圖* | ![Phase Response](figures/problem_b_phase.png)<br>*圖 4：頻率響應之相位變化與截止點* |

---

## 3. 動態範圍分析
* **輸出振幅限制**：理論最大輸出振幅 $\hat{v}_{o,max} \approx 0.947\text{ V}$，暫態模擬負向擺幅極限約為 $-0.871\text{ V}$。
* **誤差成因**：差異主因係體效應（Body Effect）推升閾值電壓 $V_{th}$ 及 $M_2$ 大訊號非線性。最大輸入信號振幅 $\hat{v}_{sig,max} \approx 27.01\text{ mV}$。

| 暫態響應輸出波形 | 非線性剪波 (Clipping) 狀態 |
| :---: | :---: |
| ![Transient](figures/problem_c_transient.png)<br>*圖 5：正常輸入下之輸出暫態響應* | ![Clipping](figures/problem_c_clipping.png)<br>*圖 6：達到最大擺幅限制時的非線性截波* |

---

## 4. 移除晶體 $M_2$ 之影響
* **電路簡化**：移除源極隨耦器 $M_2$ 後轉為標準共源極放大器。
* **增益與頻寬變化**：中頻增益降至 $\approx 25.21\text{ dB}$，高頻截止頻率顯著擴展至 $\approx 40.88\text{ MHz}$，主因是輸出端寄生電容減少。

| 移除 $M_2$ 後之電路架構 | 有無 $M_2$ 之 AC 頻寬比較 |
| :---: | :---: |
| ![No M2 Schematic](figures/problem_d_schematic.png)<br>*圖 7：標準共源極放大器電路圖* | ![No M2 AC](figures/problem_d_no_m2.png)<br>*圖 8：頻寬擴展與增益差異對比曲線* |

---

## 5. 移除 $C_S$（無源極旁路電容）之影響
* **增益崩潰**：移除 $C_S$ 引入強烈負回授，中頻增益劇降至 $\approx -1.0\text{ dB}$。
* **頻寬位移**：低頻截止頻率降至 $< 1\text{ Hz}$，高頻截止頻率大幅擴展至 $\approx 398\text{ MHz}$，展示經典的增益頻寬取捨（GBW Trade-off）。

| 無 $C_S$ 之放大器電路圖 | 增益崩潰與高頻擴展波形 |
| :---: | :---: |
| ![No CS Schematic](figures/problem_e_schematic.png)<br>*圖 9：無源極旁路電容之電路架構* | ![No CS AC](figures/problem_e_no_cs.png)<br>*圖 10：中頻衰減與極寬頻響應對比* |

---

## 6. 增加 $C_{gd}$ 對頻寬惡化之影響
* **米勒效應**：外加 $C_{gd,add} = 0.5\text{ nF}$ 雖不影響中頻增益（維持 $30.17\text{ dB}$），但使有效輸入電容激增至 $16.44\text{ nF}$。
* **高頻惡化**：高頻截止頻率 $f_{H,f}$ 嚴重惡化下降至 $\approx 2.84\text{ kHz}$，證實米勒倍增效應對高頻頻寬的宰制影響。

| 外加 $C_{gd}$ 之測試電路圖 | 高頻截止頻率嚴重衰減對比 |
| :---: | :---: |
| ![Cgd Schematic](figures/problem_f_schematic.png)<br>*圖 11：加入回授電容 $C_{gd}$ 之架構* | ![Cgd AC](figures/problem_f_added_cgd.png)<br>*圖 12：米勒效應導致高頻提早滾降之響應* |