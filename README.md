# Análisis de Señales con Transformada de Fourier

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)

## 📋 Descripción

Este proyecto implementa un análisis completo de señales utilizando la Transformada de Fourier. El código genera señales elementales (senoidal, pulso rectangular, función escalón) y visualiza su representación en los dominios del tiempo y la frecuencia, demostrando propiedades fundamentales como linealidad, desplazamiento temporal y escalamiento.

## 🚀 Características

- Generación de señales elementales:
  - Señal senoidal (5 Hz)
  - Pulso rectangular de diferentes anchos
  - Función escalón
- Cálculo de la Transformada Rápida de Fourier (FFT)
- Visualización de:
  - Dominio del tiempo
  - Espectro de magnitud
  - Espectro de fase
- Verificación de propiedades:
  - Linealidad
  - Desplazamiento temporal
  - Escalamiento (dualidad tiempo-frecuencia)

## 📊 Ejemplos de resultados

### Señal Senoidal (5 Hz)
- **Tiempo**: Onda senoidal pura
- **Frecuencia**: Picos en ±5 Hz (frecuencia única)

### Pulso Rectangular
- **Tiempo**: Pulso de duración finita
- **Frecuencia**: Espectro con forma de SINC (múltiples frecuencias)
- **Relación de incertidumbre**: Pulso más corto ↔ Espectro más ancho

### Función Escalón
- **Tiempo**: Cambio brusco en t=0.3s
- **Frecuencia**: Contiene todas las frecuencias (amplitud decreciente)

## 🔧 Requisitos e instalación

### Prerrequisitos
- Python 3.8 o superior
- Pip (gestor de paquetes de Python)

### Instalación

1. Clona este repositorio:
```bash
git clone https://github.com/TU-USUARIO/analisis-fourier-senales.git
cd analisis-fourier-senales
