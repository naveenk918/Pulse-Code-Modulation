# Pulse-Code-Modulation
# Aim
Write a simple Python program for the modulation and demodulation of PCM, and DM.
# Tools required
** Python
** Google Colab
#  Program
```python
import numpy as np
import matplotlib.pyplot as plt

# -----------------------------------
# Step 1: Generate Analog Input Signal
# -----------------------------------

fm = 2
fs = 20
duration = 2
n_bits = 3

t = np.linspace(0, duration, 1000)
ts = np.arange(0, duration, 1/fs)

x = np.sin(2 * np.pi * fm * t)
xs = np.sin(2 * np.pi * fm * ts)

# -----------------------------------
# Step 2: Quantization
# -----------------------------------

L = 2 ** n_bits
x_min = -1
x_max = 1
delta = (x_max - x_min) / L

xq = np.round((xs - x_min) / delta) * delta + x_min
xq = np.clip(xq, x_min, x_max)

# -----------------------------------
# Step 3: PCM Encoding
# -----------------------------------

indices = ((xq - x_min) / delta).astype(int)
indices = np.clip(indices, 0, L - 1)

binary_codes = [format(i, f'0{n_bits}b') for i in indices]

# -----------------------------------
# Step 4: PCM Pulse Train
# -----------------------------------

pcm_bits = ''.join(binary_codes)
pcm_wave = [int(bit) for bit in pcm_bits]
bit_time = np.arange(len(pcm_wave))

# -----------------------------------
# Step 5: PCM Decoding
# -----------------------------------

decoded_indices = np.array([int(code, 2) for code in binary_codes])
decoded_signal = decoded_indices * delta + x_min

# -----------------------------------
# Step 6: Plot without overlap
# -----------------------------------

fig, axs = plt.subplots(5, 1, figsize=(14, 18))

# Increase spacing manually
fig.subplots_adjust(hspace=0.9, top=0.95, bottom=0.06)

# -------- Plot 1 --------
axs[0].plot(t, x, lw=2)
axs[0].set_title("Original Analog Signal", pad=14)
axs[0].set_xlabel("Time")
axs[0].set_ylabel("Amplitude")
axs[0].grid(True)

# -------- Plot 2 --------
axs[1].stem(ts, xs, basefmt=" ")
axs[1].set_title("Sampled Signal", pad=14)
axs[1].set_xlabel("Time")
axs[1].set_ylabel("Amplitude")
axs[1].grid(True)

# -------- Plot 3 --------
axs[2].stem(ts, xq, basefmt=" ")
axs[2].set_title("Quantized Signal", pad=14)
axs[2].set_xlabel("Time")
axs[2].set_ylabel("Amplitude")
axs[2].grid(True)

# -------- Plot 4 --------
axs[3].step(bit_time, pcm_wave, where='post')
axs[3].set_ylim(-0.2, 1.2)
axs[3].set_title("PCM Output Waveform (Binary Pulse Train)", pad=14)
axs[3].set_xlabel("Time")
axs[3].set_ylabel("Binary")
axs[3].grid(True)

# -------- Plot 5 --------
axs[4].step(ts, decoded_signal, where='mid')
axs[4].set_title("PCM Demodulated (Reconstructed) Signal", pad=14)
axs[4].set_ylabel("Amplitude")
axs[4].grid(True)

# Only bottom graph gets X label
axs[4].set_xlabel("Time / Bit Position")

plt.show()
```
# DWaveform
<img width="1167" height="1698" alt="ex 2" src="https://github.com/user-attachments/assets/5167e63a-abd8-447c-a34b-4bc47936ec58" />

# Results
Pulse Code Modulation (PCM) converts analog signals into digital form by sampling and quantizing, resulting in a series of binary-coded pulses representing absolute amplitudes. Delta Modulation (DM), on the other hand, encodes only the change in amplitude between samples, producing a single-bit pulse stream. Thus, PCM offers higher accuracy with more bits, while **DM provides simplicity and lower bit rates.
