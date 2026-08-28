# Guía de Ejecución de Backtest - BayesianGold_XAU_Panel (2026)

Esta guía explica la estructura del robot **BayesianGold_XAU_Panel.mq5** y detalla las instrucciones paso a paso para ejecutar el backtest en **MetaTrader 5** con **ticks reales** para el período **1 de enero de 2026 al 30 de agosto de 2026**.

---

## 1. Análisis de la Estrategia del EA

- **Símbolo Principal**: `XAUUSD` (Oro / Gold).
- **Temporalidad Operativa**: `M5` (5 minutos).
- **Motor Estadístico**: Bayesian Engine combinando indicadores RSI y CCI con filtros de tendencia (EMA 50 / EMA 200 en H1) y volatilidad (ADX en H1).
- **Gestión de Riesgo y Protección**:
  - `Usar_Shield`: Detiene operaciones si la pérdida diaria supera el % configurado (`Shield_Pct = 6.0%`).
  - `Usar_FrenoRachas`: Detiene la operativa tras 2 pérdidas o break-evens seguidos (`Perdidas_Seguidas = 2`).
  - `Usar_Objetivo`: Cierre automático al alcanzar el objetivo diario (`Objetivo_Diario = 20.0%`).
  - `Usar_Breakeven` y `Usar_Spread_Max` (máximo spread permitido: 30 puntos / 3 pips).

---

## 2. Archivos Generados en la Raíz del Proyecto

1. `BayesianGold_XAU_Panel.mq5`: Código fuente completo del robot (v1.79).
2. `BayesianGold_2026.set`: Archivo de parámetros cargados para la estrategia.
3. `backtest_2026.ini`: Configuración automatizada para el Probador de Estrategias de MT5.

---

## 3. Configuración del Probador de Estrategias (Strategy Tester)

Parámetros clave configurados en `backtest_2026.ini`:

| Parámetro | Valor | Descripción |
| :--- | :--- | :--- |
| **Expert** | `BayesianGold_XAU_Panel.ex5` | EA compilado |
| **Symbol** | `XAUUSD` | Oro frente al Dólar |
| **Period** | `M5` | Marco temporal 5 minutos |
| **Model** | `2` | **Every tick based on real ticks** (Ticks reales del broker) |
| **FromDate** | `2026.01.01` | Fecha de inicio |
| **ToDate** | `2026.08.30` | Fecha de fin |
| **Deposit** | `10000 USD` | Balance inicial de prueba |
| **Leverage** | `1:500` | Apalancamiento recomendando |

---

## 4. Instrucciones de Ejecución Paso a Paso en MetaTrader 5

Para realizar la prueba de backtest en su terminal MetaTrader 5 local:

1. **Compilar el Expert Advisor**:
   - Copie `BayesianGold_XAU_Panel.mq5` en la carpeta `MQL5/Experts/` de su MetaTrader 5.
   - Abra **MetaEditor 5** (`F4`), abra el archivo y presione `F7` (**Compile**) para generar `BayesianGold_XAU_Panel.ex5`.

2. **Cargar el Preset (.set)**:
   - Copie `BayesianGold_2026.set` a la carpeta `MQL5/Profiles/Tester/` o guárdelo en su equipo.

3. **Ejecutar el Backtest mediante GUI**:
   - Abra MetaTrader 5 y presione `Ctrl + R` para abrir el **Probador de Estrategias**.
   - Seleccione `BayesianGold_XAU_Panel.ex5`.
   - Seleccione el símbolo `XAUUSD` y la temporalidad `M5`.
   - En **Modelado (Model)**, seleccione **"Every tick based on real ticks"** (*Cada tick basado en ticks reales*).
   - Defina el rango de fechas: **2026.01.01** a **2026.08.30**.
   - En la pestaña *Entradas (Inputs)*, haga clic derecho -> **Cargar (Load)** y seleccione `BayesianGold_2026.set`.
   - Haga clic en **Iniciar (Start)**.

4. **Ejecución Automatizada por Línea de Comandos (CLI)**:
   Si prefiere ejecutar MT5 desde terminal/PowerShell utilizando el archivo `.ini`:
   ```cmd
   terminal64.exe /config:backtest_2026.ini
   ```

---

## 5. Verificación de Resultados

Una vez completado el backtest, revise las siguientes pestañas del Probador de Estrategias:
- **Informe / Report**: Profit Factor, Drawdown Máximo, Sharpe Ratio y Win Rate.
- **Gráfico / Graph**: Curva de equidad y balance.
- **Operaciones / Trades**: Registro detallado de cada entrada, ejecución de Break-even, Trailing y Shield.
