![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)

## 👤 Autor

**José Ulloa**  
Ingeniero Civil Informatico, Magister en Ingenieria Informatica.

------------------------------------------------------------------------

# 📊 Alerta USDCLP Macro

Sistema cuantitativo de apoyo a decisiones para el tipo de cambio
**USD/CLP**, que integra señales macroeconómicas, análisis técnico,
modelo de Machine Learning, estacionalidad y sistema de puntuación
(0--100) con semáforo de decisión.

------------------------------------------------------------------------

## 🚀 Descripción

Este proyecto implementa un motor de decisión cuantitativo para evaluar
oportunidades de compra de dólares en Chile (USD/CLP), combinando:

-   📈 Análisis técnico (MA10, MA20, MA50, MA200 + pendiente 30 días)
-   🌎 Presión macroeconómica (DXY + US10Y − Cobre)
-   🤖 Modelo de Machine Learning (Regresión Logística, horizonte 5
    días)
-   📅 Estacionalidad por día de la semana (con detrending robusto)
-   ⏰ Ventanas horarias de liquidez
-   🎯 Sistema de score global (0--100)
-   🚦 Semáforo de decisión: COMPRAR / COMPRAR PARCIAL / ESPERAR

Incluye fallback automático desde datos intradía hacia datos diarios
cuando el intradía está desactualizado (stale).

------------------------------------------------------------------------

## 🧠 Metodología

### 1️⃣ Presión Macro

Se construye un índice:

Pressure = z(DXY) + z(US10Y) − z(Cobre)

Interpretación:

-   Presión positiva → sesgo alcista USD
-   Presión negativa → sesgo bajista USD

------------------------------------------------------------------------

### 2️⃣ Modelo de Machine Learning

-   Algoritmo: Regresión Logística balanceada
-   Horizonte: 5 días
-   Variables:
    -   Retornos 1d y 5d
    -   Desviación vs MA10
    -   Volatilidad 5d
    -   Retornos macro (Cobre, DXY, US10Y)
    -   Índice de presión

Salida: - Probabilidad de subida en 5 días

------------------------------------------------------------------------

### 3️⃣ Tendencia

Clasificación basada en:

-   MA20 / MA50 / MA200
-   Pendiente 30 días
-   Retornos 20d y 60d

Estados posibles: - ALCISTA - BAJISTA - LATERAL / MIXTA

------------------------------------------------------------------------

### 4️⃣ Estacionalidad

-   Análisis por día de la semana
-   Método robusto con detrending MA50 o MA20
-   Confianza: baja / media / alta
-   Identificación de mejor y peor día

------------------------------------------------------------------------

### 5️⃣ Score Final (0--100)

Integra:

-   Desviación MA10
-   Retorno intradía
-   Presión macro
-   ML
-   Tendencia
-   Estacionalidad
-   Liquidez horaria

Interpretación general:

-   70--100 → Condiciones favorables
-   40--69 → Condiciones mixtas
-   0--39 → Condiciones desfavorables

------------------------------------------------------------------------

## 🛠 Requisitos

Python 3.9+

Dependencias:

-   pandas
-   numpy
-   yfinance
-   scikit-learn
-   pytz

Instalación:

``` bash
pip install pandas numpy yfinance scikit-learn pytz
```

------------------------------------------------------------------------

## ▶️ Ejemplos de uso

Evaluación estándar:

``` bash
python3 alerta_usdclp_macro.py --budget 300000 --mode clp
```

Evaluación robusta recomendada:

``` bash
python3 alerta_usdclp_macro.py --budget 300000 --mode clp --season_months 9
```

Con aprendizaje de ventana horaria:

``` bash
python3 alerta_usdclp_macro.py --learn_hours
```

Forzar ejecución fuera de horario (solo simulación):

``` bash
python3 alerta_usdclp_macro.py --force
```

------------------------------------------------------------------------

## 🚦 Interpretación del semáforo

🟢 **COMPRAR**\
Condiciones favorables. Puede ejecutarse compra total o mayoritaria.

🟡 **COMPRAR PARCIAL**\
Condiciones mixtas. Recomendado dividir compra en 2--3 partes.

🔴 **ESPERAR**\
Condiciones desfavorables. Postergar compra.

------------------------------------------------------------------------

## ⚠️ Advertencia

Este sistema es una herramienta analítica y educativa.\
No constituye asesoría financiera ni recomendación de inversión.

El usuario asume toda responsabilidad por las decisiones tomadas con
base en este análisis.

