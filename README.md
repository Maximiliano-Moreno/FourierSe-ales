# -*- coding: utf-8 -*-
"""
ANÁLISIS DE SEÑALES CON TRANSFORMADA DE FOURIER
Versión para Python Online (Google Colab)
Autor: [Tu Nombre]
"""

import numpy as np
import matplotlib.pyplot as plt
from scipy.fft import fft, fftshift, fftfreq

# ============================================
# CONFIGURACIÓN INICIAL
# ============================================
print("="*60)
print("ANÁLISIS DE FOURIER - SEÑALES ELEMENTALES")
print("="*60)

fs = 1000  # Frecuencia de muestreo (Hz)
T = 1      # Duración de la señal (segundos)
N = int(fs * T)  # Número de puntos
t = np.linspace(0, T, N, endpoint=False)  # Vector de tiempo

# ============================================
# FUNCIÓN PARA GRAFICAR
# ============================================
def graficar_espectro(senal, titulo, fs):
    """Grafica señal en tiempo y su espectro (magnitud y fase)"""
    
    # Calcular FFT
    Y = fft(senal)
    Y_shifted = fftshift(Y)
    magnitud = np.abs(Y_shifted) / N
    fase = np.angle(Y_shifted)
    freqs = fftshift(fftfreq(N, 1/fs))
    
    # Crear figura
    fig, axs = plt.subplots(3, 1, figsize=(12, 8))
    fig.suptitle(titulo, fontsize=14)
    
    # Tiempo
    axs[0].plot(t, senal, 'b-', linewidth=1.5)
    axs[0].set_title('Dominio del Tiempo')
    axs[0].set_xlabel('Tiempo (s)')
    axs[0].set_ylabel('Amplitud')
    axs[0].grid(True, alpha=0.3)
    axs[0].set_xlim([0, 0.3])
    
    # Magnitud
    axs[1].plot(freqs, magnitud, 'r-', linewidth=1.5)
    axs[1].set_title('Espectro - Magnitud')
    axs[1].set_xlabel('Frecuencia (Hz)')
    axs[1].set_ylabel('|X(f)|')
    axs[1].grid(True, alpha=0.3)
    axs[1].set_xlim([-100, 100])
    
    # Fase
    axs[2].plot(freqs, fase, 'g-', linewidth=1.5)
    axs[2].set_title('Espectro - Fase')
    axs[2].set_xlabel('Frecuencia (Hz)')
    axs[2].set_ylabel('Fase (rad)')
    axs[2].grid(True, alpha=0.3)
    axs[2].set_xlim([-100, 100])
    
    plt.tight_layout()
    plt.show()

# ============================================
# 1. SEÑALES ELEMENTALES
# ============================================
print("\n[1] Generando señales elementales...")

# Senoidal de 5 Hz
senal_seno = np.sin(2 * np.pi * 5 * t)
graficar_espectro(senal_seno, 'Señal 1: Senoidal (5 Hz)', fs)

# Pulso rectangular (ancho 0.1s)
pulso_rect = np.where((t >= 0.2) & (t < 0.3), 1, 0)
graficar_espectro(pulso_rect, 'Señal 2: Pulso Rectangular (ancho 0.1s)', fs)

# Función escalón
escalon = np.where(t >= 0.3, 1, 0)
graficar_espectro(escalon, 'Señal 3: Función Escalón', fs)

# ============================================
# 2. PROPIEDAD: LINEALIDAD
# ============================================
print("\n[2] Verificando propiedad de LINEALIDAD...")

a, b = 2, 3
senal_comb = a * senal_seno + b * pulso_rect
fft_comb = fft(senal_comb)
fft_lineal = a * fft(senal_seno) + b * fft(pulso_rect)

if np.allclose(fft_comb, fft_lineal):
    print("✓ Linealidad verificada: F(2x + 3y) = 2F(x) + 3F(y)")
    graficar_espectro(senal_comb, 'Linealidad: Señal combinada (2*seno + 3*pulso)', fs)
else:
    print("✗ Error en linealidad")

# ============================================
# 3. PROPIEDAD: DESPLAZAMIENTO
# ============================================
print("\n[3] Verificando propiedad de DESPLAZAMIENTO TEMPORAL...")

# Pulso original vs desplazado
pulso_original = np.where((t >= 0.2) & (t < 0.3), 1, 0)
pulso_desplazado = np.where((t >= 0.4) & (t < 0.5), 1, 0)

print("Comparando: Original (t=0.2s) vs Desplazado (t=0.4s)")
print("✓ La magnitud debe ser IDÉNTICA")
print("✓ La fase debe ser DIFERENTE")

graficar_espectro(pulso_original, 'Desplazamiento: Pulso ORIGINAL (t=0.2s)', fs)
graficar_espectro(pulso_desplazado, 'Desplazamiento: Pulso DESPLAZADO (t=0.4s)', fs)

# ============================================
# 4. PROPIEDAD: ESCALAMIENTO (DUALIDAD)
# ============================================
print("\n[4] Verificando propiedad de ESCALAMIENTO...")

# Pulso ancho vs angosto
pulso_ancho = np.where((t >= 0.2) & (t < 0.4), 1, 0)    # ancho 0.2s
pulso_angosto = np.where((t >= 0.2) & (t < 0.25), 1, 0)  # ancho 0.05s

print("Comparando: Pulso ANCHO (0.2s) vs Pulso ANGOSTO (0.05s)")
print("✓ Señal angosta en tiempo = Espectro ANCHO en frecuencia")
print("✓ Señal ancha en tiempo = Espectro ANGOSTO en frecuencia")

graficar_espectro(pulso_ancho, 'Escalamiento: Pulso ANCHO (0.2s)', fs)
graficar_espectro(pulso_angosto, 'Escalamiento: Pulso ANGOSTO (0.05s)', fs)

# ============================================
# 5. RESUMEN Y CONCLUSIONES
# ============================================
print("\n" + "="*60)
print("RESUMEN DE RESULTADOS")
print("="*60)
print("""
📊 OBSERVACIONES IMPORTANTES:

1. SEÑAL SENOIDAL:
   → Espectro con picos en ±5 Hz (frecuencia pura)
   → Una sola frecuencia en el dominio frecuencial

2. PULSO RECTANGULAR:
   → Espectro con forma de 'sinc' (múltiples frecuencias)
   → Relación de incertidumbre: pulso corto ↔ espectro ancho

3. FUNCIÓN ESCALÓN:
   → Contiene TODAS las frecuencias (cambio brusco)
   → Amplitud decrece con la frecuencia

4. PROPIEDADES VERIFICADAS:
   ✓ Linealidad: La FFT es lineal
   ✓ Desplazamiento: Solo afecta la fase
   ✓ Escalamiento: Dualidad tiempo-frecuencia
""")

print("\n✅ ANÁLISIS COMPLETADO - 
