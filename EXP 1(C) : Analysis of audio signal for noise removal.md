# EXP 1(C) : Analysis of audio signal for noise removal
REG NO:212223060247
# AIM: 
To analyze audio signal by removing unwanted frequency.

# APPARATUS REQUIRED:  
PC installed with SCILAB. 

# PROGRAM: 
```
import numpy as np
from scipy.io import wavfile
import matplotlib.pyplot as plt
from google.colab import files
from IPython.display import Audio

# ------------------------------------------------------------
# 1️⃣ Upload Speech File
# ------------------------------------------------------------
print("📁 Please upload 'speech.wav'")
uploaded_speech = files.upload()
speech_path = list(uploaded_speech.keys())[0]  # Get uploaded file name

# ------------------------------------------------------------
# 2️⃣ Upload Noise File
# ------------------------------------------------------------
print("📁 Please upload 'noise.wav'")
uploaded_noise = files.upload()
noise_path = list(uploaded_noise.keys())[0]  # Get uploaded file name

# ------------------------------------------------------------
# 3️⃣ Load signals
# ------------------------------------------------------------
fs_s, speech = wavfile.read(speech_path)
fs_n, noise  = wavfile.read(noise_path)
assert fs_s == fs_n, "❌ Sampling rates must match!"
fs = fs_s

# Convert to float [-1, 1]
def to_float(x):
    if x.dtype.kind == 'i':  # integer type
        return x.astype(np.float32) / (np.iinfo(x.dtype).max + 1.0)
    return x.astype(np.float32)

speech = to_float(speech)
noise  = to_float(noise)

# ------------------------------------------------------------
# Convert stereo → mono if needed
# ------------------------------------------------------------
if speech.ndim > 1:
    speech = np.mean(speech, axis=1)
if noise.ndim > 1:
    noise = np.mean(noise, axis=1)

# ------------------------------------------------------------
# Make sure both are same length
# ------------------------------------------------------------
L = min(len(speech), len(noise))
speech = speech[:L]
noise  = noise[:L]

# ------------------------------------------------------------
# 4️⃣ Mix the signals (adjust alpha for noise strength)
# ------------------------------------------------------------
alpha = 0.5
mixed = speech + alpha * noise

# ------------------------------------------------------------
# 5️⃣ FFT-based filtering
# ------------------------------------------------------------
N = int(2 ** np.ceil(np.log2(L)))  # next power of two
M = np.fft.rfft(mixed, n=N)
freqs = np.fft.rfftfreq(N, 1/fs)

# Band-pass mask for speech (300 Hz – 3400 Hz)
low, high = 300.0, 3400.0
mask = np.logical_and(freqs >= low, freqs <= high).astype(float)
M_filtered = M * mask

# Inverse FFT to reconstruct
recovered = np.fft.irfft(M_filtered, n=N)[:L]
recovered = recovered / (np.max(np.abs(recovered)) + 1e-12)

# ------------------------------------------------------------
# 6️⃣ Save output audio
# ------------------------------------------------------------
wavfile.write('recovered_simple.wav', fs, (recovered * 32767).astype(np.int16))
print("✅ Recovered audio saved as: recovered_simple.wav")

# ------------------------------------------------------------
# 7️⃣ Plot waveforms
# ------------------------------------------------------------
t = np.arange(L) / fs

plt.figure(figsize=(12, 8))
plt.subplot(3, 1, 1)
plt.plot(t, speech)
plt.title("Original Speech Signal")
plt.xlabel("Time [s]"); plt.ylabel("Amplitude")

plt.subplot(3, 1, 2)
plt.plot(t, mixed)
plt.title("Noisy Speech (Speech + Noise)")
plt.xlabel("Time [s]")

plt.subplot(3, 1, 3)
plt.plot(t, recovered)
plt.title("Recovered Speech after FFT Filtering")
plt.xlabel("Time [s]")
plt.tight_layout()
plt.show()

# ------------------------------------------------------------
# 8️⃣ Plot Magnitude Spectra
# ------------------------------------------------------------
plt.figure(figsize=(10,5))
plt.semilogy(freqs, np.abs(np.fft.rfft(mixed, n=N)), label='Noisy Speech')
plt.semilogy(freqs, np.abs(M_filtered), label='Filtered Spectrum')
plt.xlim(0, 8000)
plt.legend()
plt.xlabel('Frequency (Hz)')
plt.title('Magnitude Spectra')
plt.grid(True)
plt.show()

# ------------------------------------------------------------
# 9️⃣ Listen to audio
# ------------------------------------------------------------
print("🎧 Original Speech:")
display(Audio(speech, rate=fs))
print("🎧 Noisy (Mixed) Speech:")
display(Audio(mixed, rate=fs))
print("🎧 Recovered Speech:")
display(Audio(recovered, rate=fs))
```
#OUTPUT:
<img width="1238" height="829" alt="image" src="https://github.com/user-attachments/assets/439b893d-d821-497f-a5dd-321c557fb168" />

<img width="1362" height="827" alt="image" src="https://github.com/user-attachments/assets/dd9f6b7c-042e-4db8-aef6-5262e60c0f9f" />



# RESULT: 
Thus the analyze audio signal by removing unwanted frequency is done successfully
