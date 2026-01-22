/**
 * MHCh-450L 氫氣瓶存量計算器
 * 
 * 基於 P-C-T (Pressure-Composition-Temperature) 特性曲線
 * 金屬氫化物儲氫罐: MmNiMnCo (AB5-type)
 * 
 * 規格:
 * - 最大儲氫量: 450 NL (Normal Liters)
 * - 工作壓力: 0 ~ 1 MPaG @ 20°C
 * - 工作溫度: 0 ~ 40°C
 */

// P-C-T 曲線數據 (基於規格書的脫附曲線 - Desorption)
// 壓力 (MPaA) vs 氫氣含量 (Ncc/g) 在不同溫度下
// 注意: MPaA = MPaG + 0.1 (絕對壓力 = 表壓 + 大氣壓)

interface PCTDataPoint {
  pressure: number; // MPaA
  content: number;  // Ncc/g (相對含量百分比)
}

// 溫度對應的 P-C-T 脫附曲線 (簡化的線性內插數據)
const PCT_CURVES: Record<number, PCTDataPoint[]> = {
  0: [
    { pressure: 0.05, content: 5 },
    { pressure: 0.08, content: 20 },
    { pressure: 0.10, content: 50 },
    { pressure: 0.12, content: 100 },
    { pressure: 0.15, content: 150 },
    { pressure: 0.20, content: 165 },
    { pressure: 0.50, content: 170 },
    { pressure: 1.00, content: 175 },
  ],
  10: [
    { pressure: 0.08, content: 5 },
    { pressure: 0.12, content: 20 },
    { pressure: 0.15, content: 50 },
    { pressure: 0.18, content: 100 },
    { pressure: 0.25, content: 150 },
    { pressure: 0.35, content: 165 },
    { pressure: 0.60, content: 170 },
    { pressure: 1.00, content: 175 },
  ],
  20: [
    { pressure: 0.12, content: 5 },
    { pressure: 0.18, content: 20 },
    { pressure: 0.22, content: 50 },
    { pressure: 0.28, content: 100 },
    { pressure: 0.40, content: 150 },
    { pressure: 0.55, content: 165 },
    { pressure: 0.80, content: 170 },
    { pressure: 1.00, content: 172 },
  ],
  30: [
    { pressure: 0.20, content: 5 },
    { pressure: 0.28, content: 20 },
    { pressure: 0.35, content: 50 },
    { pressure: 0.45, content: 100 },
    { pressure: 0.60, content: 150 },
    { pressure: 0.80, content: 165 },
    { pressure: 1.00, content: 168 },
    { pressure: 1.20, content: 170 },
  ],
  40: [
    { pressure: 0.30, content: 5 },
    { pressure: 0.42, content: 20 },
    { pressure: 0.52, content: 50 },
    { pressure: 0.68, content: 100 },
    { pressure: 0.90, content: 150 },
    { pressure: 1.10, content: 162 },
    { pressure: 1.30, content: 165 },
    { pressure: 1.50, content: 167 },
  ],
};

// MHCh-450L 規格常數
export const MHCH_450L_SPECS = {
  model: 'MHCh-450L',
  maxCapacity: 450, // NL (Normal Liters)
  maxPressure: 500, // psi (滿瓶)
  minPressure: 0, // psi
  maxTemp: 40, // °C
  minTemp: 0, // °C
  typicalDischargeRate: 560, // Ncc/min
  weight: 4.7, // kg
  dimensions: 'O.D.81 x L270 mm',
  material: 'Aluminum Alloy, A6061-T6',
};

// 單位轉換常數
const PSI_TO_MPA = 0.00689476; // 1 psi = 0.00689476 MPa

/**
 * 線性內插
 */
function linearInterpolate(
  x: number,
  x1: number,
  x2: number,
  y1: number,
  y2: number
): number {
  return y1 + ((x - x1) * (y2 - y1)) / (x2 - x1);
}

/**
 * 從 P-C-T 曲線獲取氫氣含量 (Ncc/g)
 */
function getContentFromPCT(pressureMPaA: number, curve: PCTDataPoint[]): number {
  // 邊界檢查
  if (pressureMPaA <= curve[0].pressure) {
    return curve[0].content;
  }
  if (pressureMPaA >= curve[curve.length - 1].pressure) {
    return curve[curve.length - 1].content;
  }

  // 找到內插區間
  for (let i = 0; i < curve.length - 1; i++) {
    if (pressureMPaA >= curve[i].pressure && pressureMPaA <= curve[i + 1].pressure) {
      return linearInterpolate(
        pressureMPaA,
        curve[i].pressure,
        curve[i + 1].pressure,
        curve[i].content,
        curve[i + 1].content
      );
    }
  }

  return 0;
}

/**
 * 計算氫氣剩餘量
 * @param pressurePSI - 當前表壓 (psi)
 * @param temperatureC - 當前溫度 (°C)
 * @returns 氫氣剩餘量相關數據
 */
export function calculateHydrogenRemaining(
  pressurePSI: number,
  temperatureC: number
): {
  remainingNL: number;
  remainingPercent: number;
  estimatedRuntime: number; // 分鐘
  status: 'full' | 'normal' | 'low' | 'critical';
} {
  // 將 psi 轉換為 MPaG，再轉為絕對壓力 MPaA
  const pressureMPaG = pressurePSI * PSI_TO_MPA;
  const pressureMPaA = pressureMPaG + 0.1;

  // 限制溫度範圍
  const clampedTemp = Math.max(0, Math.min(40, temperatureC));

  // 找到最近的溫度曲線進行內插
  const temps = Object.keys(PCT_CURVES).map(Number).sort((a, b) => a - b);
  let lowerTemp = temps[0];
  let upperTemp = temps[temps.length - 1];

  for (let i = 0; i < temps.length - 1; i++) {
    if (clampedTemp >= temps[i] && clampedTemp <= temps[i + 1]) {
      lowerTemp = temps[i];
      upperTemp = temps[i + 1];
      break;
    }
  }

  // 獲取兩個溫度下的含量
  const contentLower = getContentFromPCT(pressureMPaA, PCT_CURVES[lowerTemp]);
  const contentUpper = getContentFromPCT(pressureMPaA, PCT_CURVES[upperTemp]);

  // 溫度內插
  let content: number;
  if (lowerTemp === upperTemp) {
    content = contentLower;
  } else {
    content = linearInterpolate(
      clampedTemp,
      lowerTemp,
      upperTemp,
      contentLower,
      contentUpper
    );
  }

  // 最大含量 (滿載時的 Ncc/g)
  const maxContent = 170; // 約在平台區的最大值

  // 計算剩餘百分比和 NL
  const remainingPercent = Math.max(0, Math.min(100, (content / maxContent) * 100));
  const remainingNL = (remainingPercent / 100) * MHCH_450L_SPECS.maxCapacity;

  // 預估運行時間 (基於典型放電速率 560 Ncc/min)
  const estimatedRuntime = (remainingNL * 1000) / MHCH_450L_SPECS.typicalDischargeRate;

  // 狀態判斷
  let status: 'full' | 'normal' | 'low' | 'critical';
  if (remainingPercent >= 80) {
    status = 'full';
  } else if (remainingPercent >= 40) {
    status = 'normal';
  } else if (remainingPercent >= 15) {
    status = 'low';
  } else {
    status = 'critical';
  }

  return {
    remainingNL: Math.round(remainingNL * 10) / 10,
    remainingPercent: Math.round(remainingPercent * 10) / 10,
    estimatedRuntime: Math.round(estimatedRuntime),
    status,
  };
}

/**
 * 計算公式說明
 */
export const FORMULA_EXPLANATION = `
## MHCh-450L 氫氣存量計算公式

### 基本原理
金屬氫化物儲氫罐遵循 P-C-T (壓力-成分-溫度) 特性曲線。

### 計算步驟
1. **壓力轉換**: P(MPaG) = P(psi) × 0.00689476，P(MPaA) = P(MPaG) + 0.1
2. **P-C-T 查表**: 根據溫度選擇對應曲線，由壓力查得氫氣含量 (Ncc/g)
3. **容量計算**: 
   - 剩餘百分比 = (當前含量 / 最大含量) × 100%
   - 剩餘容量 (NL) = 剩餘百分比 × 450 NL
4. **運行時間**: 預估時間 = 剩餘容量 / 放電速率 (560 Ncc/min)

### 簡化公式
對於 20°C 環境下的近似計算：

\`\`\`
剩餘量 (NL) ≈ 450 × f(P)
\`\`\`

其中 f(P) 為壓力相關函數 (P 單位: psi):
- P ≤ 15 psi: f(P) ≈ 0 ~ 0.1
- 15 < P < 60 psi: f(P) ≈ 0.1 + (P - 15) × 0.017 (線性區)
- P ≥ 60 psi: f(P) ≈ 0.85 ~ 1.0 (平台區)

### 單位換算
- 1 psi = 0.00689476 MPa
- 滿瓶壓力 500 psi ≈ 3.45 MPa

### 注意事項
- 低溫會降低平衡壓力，需要更高壓力才能釋放相同氫量
- 溫度低於 20°C 時，可釋放的氫氣量會減少
- 建議在 20~40°C 環境下使用以獲得最佳性能
`;
