# Resumen del Proyecto: Valoración de Derivados Financieros (FX, Commodities y Acciones)

Este proyecto implementa y compara modelos fundamentales de valoración de derivados (Modelo Binomial, Black-Scholes-Merton y Monte Carlo) utilizando Python y datos reales extraídos de Bloomberg.

## 1. Módulo 1: Opciones sobre Commodities (Futuro de Cobre) 銅

Este módulo verificó la convergencia del Modelo Binomial con la solución analítica de Black-76 (BSM para futuros).

### Datos de Mercado (Bloomberg)

| Parámetro | Valor | Comentarios |
| :--- | :---: | :--- |
| **Futuro/Strike (F=K)** | **$502.45$** USD | Contrato de Cobre Dec'25 (ATMF). |
| **Volatilidad ($\mathbf{\sigma}$)** | **$25.387\%$** | Volatilidad Implícita Anual. |
| **Vencimiento ($\mathbf{T}$)** | $\approx \mathbf{0.09041}$ años (33 días) | Plazo muy corto. |
| **Tasa Libre de Riesgo ($\mathbf{r}$)** | $\mathbf{3.985\%}$ | USD SOFR. |
| **Prima Bloomberg (Target)** | $\mathbf{15.559}$ Prc | Precio por unidad de contrato. |

### Conclusiones de Valoración

El modelo demostró la equivalencia teórica ($\mathbf{V_{RN} = V_{RP}}$) y la convergencia a la solución BSM.

| Métrica | Valor Binomial ($\mathbf{N=1}$) | Valor Convergente ($\mathbf{N=10000}$) | Valor BSM |
| :---: | :---: | :---: | :---: |
| **Precio por Unidad** | $\mathbf{19.0989}$ USD | $\mathbf{15.2421}$ USD | $\mathbf{15.2424}$ USD |
| **Prima Total (25k uds.)** | — | $\mathbf{\$381,061.18}$ USD | $\mathbf{\$388,975.00}$ USD (Target) |
| **Diferencia con Target** | — | **$-2.04\%$** | **$-2.04\%$** |

**Hallazgo Clave:** El modelo $\mathbf{N=1}$ arrojó un precio inicial inexacto ($\mathbf{19.0989}$ USD), pero el precio convergió rápidamente a $\mathbf{15.2421}$ USD a $\mathbf{N \approx 1000}$ pasos, verificando la robustez de la valoración.

---

## 2. Módulo 2: Opciones de Tipo de Cambio (EUR/USD) 💵

Este módulo implementó el modelo **Black-Scholes-Merton (BSM) para FX** y analizó la Delta, la Paridad Put-Call y las estrategias complejas.

### Datos de Mercado (Bloomberg)

| Parámetro | Valor |
| :--- | :---: |
| **Spot ($\mathbf{S}$)** | $\mathbf{1.1594}$ |
| **Strike ATM ($\mathbf{K}$)** | $\mathbf{1.1654}$ |
| **Volatilidad ($\mathbf{\sigma}$)** | $\mathbf{6.328\%}$ |
| **Tasa Doméstica ($\mathbf{r_d}$)** | $\mathbf{3.845\%}$ (USD) |
| **Tasa Extranjera ($\mathbf{r_f}$)** | $\mathbf{1.870\%}$ (EUR) |
| **Vencimiento ($\mathbf{T}$)** | $\mathbf{0.25}$ años (3 meses) |

### Conclusiones de Estructuras y Estrategia

| Estructura | Valor Calculado | $\mathbf{\Delta}$ Compuesta | Estrategia (Tipo) |
| :---: | :---: | :---: | :---: |
| **Call Vanilla** | $\mathbf{0.0145}$ USD | $\mathbf{0.5033}$ | Direccional (Larga) |
| **Forward Largo** | $\mathbf{0.0004}$ USD | $\mathbf{0.9953}$ | Direccional (Bloqueo de Precio) |
| **Call Spread** | $\mathbf{0.0128}$ USD | $\mathbf{0.3198}$ | Direccional Moderada |
| **Straddle Largo** | $\mathbf{0.0291}$ USD | $\mathbf{0.0220}$ | Volatilidad (Neutral Dirección) |

**Propiedad de la Delta:** Se verificó la propiedad fundamental de adición: $\mathbf{\Delta_{Estructura} = \sum \Delta_{Componentes}}$. Esta propiedad $\mathbf{SIMPLIFICA}$ la gestión de riesgo al permitir el **Delta-Hedging** de todo el portafolio.

**Análisis de Greeks (Call vs Put):** Solo **Delta** ($\mathbf{\Delta}$) y **Rho** ($\mathbf{\rho}$) difieren en signo, reflejando el sesgo direccional y la sensibilidad a la tasa de descuento ($\mathbf{r_d}$).

---

## 3. Módulo 3: Opciones sobre Acciones (AAPL) con Monte Carlo 📈

Este módulo valoró opciones exóticas utilizando la simulación de Monte Carlo (GBM).

### Parámetros de Simulación

| Parámetro | Valor |
| :--- | :---: |
| **Spot ($\mathbf{S_0}$)** | $\mathbf{262.82}$ USD |
| **Volatilidad ($\mathbf{\sigma}$)** | $\mathbf{24.353\%}$ |
| **Tasa Libre de Riesgo ($\mathbf{r}$)** | $\mathbf{3.850\%}$ |
| **Rend. Dividendo ($\mathbf{q}$)** | $\mathbf{0.401\%}$ |
| **Simulaciones ($\mathbf{M}$)** | $\mathbf{50,000}$ |
| **Barreras (DKO)** | $\mathbf{210.26} / \mathbf{315.38}$ USD |

### Conclusiones de Opciones Exóticas

| Opción | Prima (MC) | Path-Dependent | Interpretación (Costo Relativo) |
| :---: | :---: | :---: | :--- |
| **Lookback Call** | $\mathbf{23.7088}$ USD | **SÍ** | **La más cara**. Otorga el derecho de comprar al mínimo histórico. |
| **Call Vanilla** | $\mathbf{13.6658}$ USD | **NO** | Precio de referencia BSM. |
| **Asiática Call** | $\mathbf{7.8343}$ USD | **SÍ** | **Más barata** que Vanilla; el promedio suaviza la volatilidad. |
| **Knock-Out (DKO)**| $\mathbf{7.1894}$ USD | **SÍ** | **Descuento del 47.3%** sobre la Vanilla (por aceptar anulación). |
| **Call Binaria** | $\mathbf{0.4986}$ USD | **NO** | Prima ≈ $\mathbf{50\%}$ de probabilidad descontada de terminar ITM. |
| **Exchange** | $\mathbf{0.0774}$ USD | **NO** | Apuesta por el rendimiento relativo ($\mathbf{S_1 / S_2}$). |

**Análisis del Histograma:** El histograma de rendimientos mostró **leptocurtosis** (colas pesadas), lo que justifica el uso de Monte Carlo y el sobreprecio que el mercado aplica a opciones que cubren movimientos extremos (Put OTM). La simulación de Monte Carlo es la herramienta correcta para valorar estas opciones cuya complejidad excede el modelo BSM.

A continuación algunos de los datos directos de la terminal de Bloomberg que fueron implementados. 


**Opción Vanilla AAPL**

<img width="803" height="504" alt="Captura de pantalla 2025-10-26 a la(s) 3 04 08 p m" src="https://github.com/user-attachments/assets/d9b9551d-112c-4233-9b53-af102d8c4f13" />

**Smile AAPL**

<img width="860" height="534" alt="Captura de pantalla 2025-10-26 a la(s) 3 00 26 p m" src="https://github.com/user-attachments/assets/9a7466ae-1840-42cf-86b7-be905400889f" />

**Futuro del Cobre**

<img width="307" height="449" alt="Captura de pantalla 2025-10-26 a la(s) 4 06 22 p m" src="https://github.com/user-attachments/assets/5a51a6a6-ea02-4ada-89ca-67001dec78e7" />

**EURUSD**

<img width="289" height="449" alt="Captura de pantalla 2025-10-26 a la(s) 4 06 29 p m" src="https://github.com/user-attachments/assets/7950103b-5e31-4219-a433-f3f21d08fddb" />
