# Pulse Code Modulation
# Aim
To generate a Pulse Code Modulation (PCM) signal by sampling, quantizing, encoding, and decoding an analog sinusoidal input using Python.
# Tools required
- Python
- Google Colab
- NumPy Library
- Matplotlib Library
- Computer / Laptop
# Theory 
Pulse Code Modulation (PCM) is a widely used digital modulation technique that converts analog signals into digital form for reliable transmission and storage. In this method, a continuous-time analog signal is first converted into a discrete-time signal through sampling. According to the Nyquist Theorem, the sampling frequency must be at least twice the maximum frequency component of the signal to avoid aliasing and ensure accurate reconstruction at the receiver. This sampled signal still has continuous amplitude values, which are then processed further.

The next step is quantization, where each sampled amplitude is approximated to the nearest value from a finite set of levels. This process introduces a small difference between the actual and approximated values, known as quantization error or noise. Quantization can be uniform or non-uniform, depending on how the levels are distributed. After quantization, encoding is performed, where each quantized level is assigned a unique binary code. This results in a stream of binary data that represents the original analog signal and can be easily transmitted through digital communication systems.

PCM systems also include processes at the receiver side, where the digital signal is decoded, reconstructed, and filtered to recover the original analog signal. The quality of the reconstructed signal depends on factors such as sampling rate, number of quantization levels, and channel conditions. PCM offers several advantages, including high noise immunity, ease of multiplexing, and compatibility with modern digital systems. However, it requires a larger bandwidth and involves more complex circuitry compared to analog communication methods. Due to its reliability and efficiency, PCM is extensively used in applications such as digital telephony, audio recording, compact discs, and satellite communication systems.
#  Program
```python
import numpy as np
import matplotlib.pyplot as plt
# Step 1: Generate Analog Input Signal
fm = 2
fs = 20
duration = 2
n_bits = 3

t = np.linspace(0, duration, 1000)
ts = np.arange(0, duration, 1/fs)

x = np.sin(2 * np.pi * fm * t)
xs = np.sin(2 * np.pi * fm * ts)
# Step 2: Quantization
L = 2 ** n_bits
x_min = -1
x_max = 1
delta = (x_max - x_min) / L

xq = np.round((xs - x_min) / delta) * delta + x_min
xq = np.clip(xq, x_min, x_max)
# Step 3: PCM Encoding

indices = ((xq - x_min) / delta).astype(int)
indices = np.clip(indices, 0, L - 1)

binary_codes = [format(i, f'0{n_bits}b') for i in indices]
# Step 4: PCM Pulse Train

pcm_bits = ''.join(binary_codes)
pcm_wave = [int(bit) for bit in pcm_bits]
bit_time = np.arange(len(pcm_wave))

# Step 5: PCM Decoding

decoded_indices = np.array([int(code, 2) for code in binary_codes])
decoded_signal = decoded_indices * delta + x_min

# Step 6: Plot without overlap
fig, axs = plt.subplots(5, 1, figsize=(14, 18))

# Increase spacing manually
fig.subplots_adjust(hspace=0.9, top=0.95, bottom=0.06)

# Plot 1 
axs[0].plot(t, x, lw=2)
axs[0].set_title("Original Analog Signal", pad=14)
axs[0].set_xlabel("Time")
axs[0].set_ylabel("Amplitude")
axs[0].grid(True)

# Plot 2 
axs[1].stem(ts, xs, basefmt=" ")
axs[1].set_title("Sampled Signal", pad=14)
axs[1].set_xlabel("Time")
axs[1].set_ylabel("Amplitude")
axs[1].grid(True)

# Plot 3 
axs[2].stem(ts, xq, basefmt=" ")
axs[2].set_title("Quantized Signal", pad=14)
axs[2].set_xlabel("Time")
axs[2].set_ylabel("Amplitude")
axs[2].grid(True)

# Plot 4
axs[3].step(bit_time, pcm_wave, where='post')
axs[3].set_ylim(-0.2, 1.2)
axs[3].set_title("PCM Output Waveform (Binary Pulse Train)", pad=14)
axs[3].set_xlabel("Time")
axs[3].set_ylabel("Binary")
axs[3].grid(True)

# Plot 5
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
The analog input signal was successfully converted into PCM binary pulses and reconstructed at the receiver side with quantization levels corresponding to 3-bit encoding.
