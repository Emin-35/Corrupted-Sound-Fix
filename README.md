# ReadMe for Sound File Analysis and Restoration
## Overview
This project involves analyzing and cleaning a corrupted sound file using Python. The code performs the following tasks:

1. Loads and visualizes the spectrum of a normal sound file.
2. Loads and visualizes the spectrum of a corrupted sound file.
3. Cleans the corrupted sound by removing frequencies above a certain threshold.
4. Provides an alternative solution by subtracting the corrupted sound from the normal sound to obtain a cleaner version.

## Libraries Used
dsp: Custom or domain-specific library for digital signal processing.
matplotlib.pyplot: For plotting waveforms and spectrums.
pydub: For handling audio file operations and performing audio manipulations.

```bash
!wget https://github.com/AllenDowney/ThinkDSP/raw/master/code/thinkdsp.py
import thinkdsp as dsp
import numpy as np
import matplotlib.pyplot as plt
from pydub import AudioSegment
```

# Detailed Explanation
## Part 1: Spectrum Analysis
Loading and Plotting the Normal Sound File Spectrum:

```python
wave_nor = dsp.read_wave('original_sound.wav')
nor_spectrum = wave_nor.make_spectrum()
nor_spectrum.plot()
```

'wave_nor': Reads the normal sound file.

'nor_spectrum': Converts the waveform to its frequency spectrum.

'nor_spectrum.plot()': Plots the frequency spectrum of the normal sound.

## Loading and Plotting the Corrupted Sound File Spectrum:

```python
wave_corr = dsp.read_wave('corrupted_sound.wav')
corr_spectrum = wave_corr.make_spectrum()
corr_spectrum.plot()
```

'wave_corr': Reads the corrupted sound file.

'corr_spectrum': Converts the waveform to its frequency spectrum.

'corr_spectrum.plot()': Plots the frequency spectrum of the corrupted sound.

## Part 2: Waveform Visualization and Playback
Plotting and Playing the Corrupted Sound:

```python
ys_corr = wave_corr.ys
plt.plot(ys_corr)
plt.show()
wave_corr.make_audio()
```

'ys_corr': Extracts the waveform data from the corrupted sound.

'plt.plot(ys_corr)': Plots the waveform.

'wave_corr.make_audio()': Plays the corrupted sound.

```python
ys_nor = wave_nor.ys
plt.plot(ys_nor)
plt.show()
wave_nor.make_audio()
```

'ys_nor': Extracts the waveform data from the normal sound.

'plt.plot(ys_nor)': Plots the waveform.

'wave_nor.make_audio()': Plays the normal sound.

## Part 3: Cleaning the Corrupted Sound
Removing High Frequencies from the Corrupted Sound

```python
corr_spectrum.hs[12500:] = 0.
plt.plot(corr_spectrum.hs)
clean_wave = corr_spectrum.make_wave()
clean_wave.make_audio()
```

'corr_spectrum.hs[12500:] = 0.': Removes frequencies above 12,500 Hz from the corrupted sound's spectrum.

'plt.plot(corr_spectrum.hs)': Plots the modified spectrum.

'clean_wave = corr_spectrum.make_wave()': Converts the modified spectrum back to a waveform.

'clean_wave.make_audio()': Plays the cleaned sound.

## Part 4: Alternative Cleaning Solution
Subtracting Sounds to Clean the Corrupted Sound:

```python
def subtract_sounds(input_file_1, input_file_2, output_file):
    sound_1 = AudioSegment.from_file(input_file_1, format='wav')
    sound_2 = AudioSegment.from_file(input_file_2, format='wav')
    result = sound_1.overlay(sound_2, loop=True, times=1, position=0, gain_during_overlay=-500)
    result.export(output_file, format='wav')

subtract_sounds('corrupted_sound.wav', 'original_sound.wav', 'clear_sound.wav')

wave_clear = dsp.read_wave('clear_sound.wav')
wave_clear.make_audio()
```

# Conclusion
This project provides two methods for cleaning a corrupted sound file: Frequency spectrum modification and sound subtraction. Each method has its use cases and effectiveness depending on the nature of the corruption. The provided code and steps can be adjusted and extended based on specific requirements and the characteristics of the sound files involved.

