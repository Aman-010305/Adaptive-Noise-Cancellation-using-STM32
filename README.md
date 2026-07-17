# Adaptive-Noise-Cancellation-using-STM32
An LMS-based Adaptive Noise Cancellation system for speech enhancement using MATLAB and STM32F407VG. The system dynamically removes background noise using dual-microphone inputs, achieving an SNR improvement of 6.27 dB and stable convergence, making it suitable for real-time embedded DSP applications.

# Overview
In real-world environments, speech signals are often corrupted by unwanted background noise originating from traffic, machinery, crowded places, and electronic interference. This degradation significantly affects the performance of communication systems, hearing aids, voice assistants, teleconferencing platforms, and speech recognition applications.

This project presents an Adaptive Noise Cancellation (ANC) system based on the Least Mean Squares (LMS) adaptive filtering algorithm to enhance speech quality in dynamically changing noise environments. Unlike conventional fixed filters, the LMS algorithm continuously updates its filter coefficients in real time, enabling effective suppression of non-stationary background noise.

The proposed system employs a dual-input microphone architecture, where:

The primary input contains the desired speech signal mixed with ambient noise.
The reference input captures only the noise component.

Using the LMS adaptation process, the system estimates the noise signal and subtracts it from the corrupted speech, resulting in a cleaner and more intelligible output signal.

The project was developed and evaluated in MATLAB, with performance assessed using Signal-to-Noise Ratio (SNR), Mean Square Error (MSE), waveform analysis, Power Spectral Density (PSD), and spectrogram analysis. Experimental results demonstrated a significant SNR improvement of 6.27 dB, increasing the speech quality from 0.12 dB to 6.39 dB, while achieving a low steady-state error of 7.23 × 10⁻⁵.

## 🛠️ Tools Used

### MATLAB
MATLAB was used as the primary development and simulation environment for implementing the LMS adaptive filtering algorithm. It was utilized for generating noisy speech signals, adaptive filter design, performance evaluation, and visualization through waveform, power spectral density (PSD), and spectrogram analysis.

### STM32F407VG DISC1
The STM32F407VG DISC1 development board, based on the ARM Cortex-M4 processor with a floating-point unit (FPU), was selected as the target hardware platform for real-time implementation of the adaptive noise cancellation system.

### STM32CubeIDE
STM32CubeIDE was used for firmware development, debugging, code compilation, and integration of DSP algorithms on the STM32 microcontroller.

### STM32CubeMX
STM32CubeMX was employed for peripheral configuration and automatic code generation, simplifying the setup of hardware resources required for embedded implementation.

### ARM CMSIS-DSP Library
The ARM CMSIS-DSP library provides optimized digital signal processing functions for Cortex-M processors. The `arm_lms_norm_f32` function can be utilized for efficient real-time implementation of the LMS adaptive filtering algorithm.

---

## ⚙️ Techniques Used

### Least Mean Squares (LMS) Adaptive Filtering
The LMS algorithm is an iterative adaptive filtering technique that continuously updates filter coefficients to minimize the error between the desired signal and the filter output. Its low computational complexity and stable convergence make it highly suitable for real-time noise cancellation applications.

### Adaptive Noise Cancellation (ANC)
Adaptive Noise Cancellation employs a dual-input approach in which one signal contains speech corrupted by noise while another provides a reference noise input. The adaptive filter estimates the noise component and subtracts it from the primary signal to recover clean speech.

### Digital Signal Processing (DSP)
DSP techniques were applied for speech signal acquisition, preprocessing, filtering, and analysis in both time and frequency domains.

### Performance Evaluation
The effectiveness of the proposed system was evaluated using:
- **Signal-to-Noise Ratio (SNR)** to quantify speech enhancement performance.
- **Mean Square Error (MSE)** to analyze convergence and filter accuracy.

### Signal Analysis Techniques
Time-domain waveform analysis, Welch Power Spectral Density (PSD), and spectrogram analysis were used to visually verify noise suppression and preservation of important speech components.

## IV. WORKING METHODOLOGY

The proposed Adaptive Noise Cancellation (ANC) system is developed using the Least Mean Squares (LMS) adaptive filtering algorithm to enhance speech quality in noisy environments. The complete implementation consists of two stages: MATLAB-based simulation and real-time embedded implementation on the STM32F407VG DISC1 microcontroller.

### A. Audio Acquisition and Signal Generation

Initially, a clean speech audio signal and a separate noise signal are imported into MATLAB. The noise signal may consist of white noise, Gaussian noise, or environmental noise recordings. These two signals are combined to generate a **primary input signal**, represented as

\[
d(n)=s(n)+v(n)
\]

where:

- \(s(n)\) = desired clean speech signal
- \(v(n)\) = unwanted background noise

A reference noise signal \(x(n)\), correlated with the noise present in the primary signal, is also provided to the adaptive filter.

### B. LMS Adaptive Filtering

The LMS adaptive filter continuously updates its filter coefficients to estimate the noise component present in the corrupted speech signal. The filter output is given by

\[
y(n)=w^T(n)x(n)
\]

where:

- \(w(n)\) = adaptive filter coefficient vector
- \(x(n)\) = reference noise input

The error signal, which represents the recovered speech signal, is calculated as

\[
e(n)=d(n)-y(n)
\]

The filter coefficients are iteratively updated using the LMS weight update equation:

\[
w(n+1)=w(n)+2\mu e(n)x(n)
\]

where \(\mu\) is the step size parameter controlling the convergence speed and stability of the algorithm.

Through successive iterations, the filter minimizes the mean square error and effectively suppresses background noise while preserving the desired speech components.

### C. Conversion to Embedded Format

For hardware implementation, the audio samples are converted into header (`.h`) files containing numerical sample arrays. This conversion enables the microcontroller to directly access the audio data in a machine-readable format.

Python scripts are utilized for importing audio files, converting them into C-compatible arrays, and exporting the processed data. Python provides flexibility since it does not impose significant limitations on audio file handling.

### D. Real-Time Implementation on STM32

The embedded implementation is carried out on the **STM32F407VG DISC1** development board based on the ARM Cortex-M4 processor.

The ARM CMSIS-DSP library provides optimized implementations of adaptive filtering algorithms. The LMS filtering operation is performed using the built-in normalized LMS function:

```c
arm_lms_norm_f32()
```

This function performs adaptive coefficient updates efficiently and is highly suitable for real-time DSP applications on resource-constrained embedded systems.

Due to memory limitations of the STM32 platform, audio duration is restricted to approximately **3 seconds** for real-time processing. After compiling and uploading the firmware using STM32CubeIDE, the processed speech output can be obtained through the onboard audio interface or audio jack, where the background noise is significantly reduced.

### E. Performance Evaluation

The effectiveness of the proposed system is evaluated using both objective and visual performance metrics.

#### 1) Signal-to-Noise Ratio (SNR)

The improvement in speech quality is quantified using Signal-to-Noise Ratio (SNR). Experimental results showed:

- Input SNR = **0.12 dB**
- Output SNR = **6.39 dB**
- SNR Improvement = **6.27 dB**

The considerable increase in SNR demonstrates effective suppression of background noise.

#### 2) Mean Square Error (MSE)

The convergence behavior of the adaptive filter is evaluated using Mean Square Error (MSE). The LMS algorithm achieved a steady-state error of

\[
\text{SS-MSE}=7.23\times10^{-5}
\]

indicating stable convergence and accurate estimation of the noise component.

#### 3) Time-Domain Analysis

Waveform comparisons between the original speech, noisy speech, and filtered output indicate that the recovered signal closely resembles the clean speech waveform with significantly reduced noise components.

#### 4) Frequency-Domain Analysis

Welch Power Spectral Density (PSD) analysis confirms attenuation of unwanted frequency components while preserving important speech frequencies.

#### 5) Spectrogram Analysis

Spectrogram comparisons further verify successful suppression of background noise without introducing noticeable spectral distortion, thereby ensuring high-fidelity speech reconstruction.

Overall, the proposed LMS-based Adaptive Noise Cancellation system demonstrates low computational complexity, reliable convergence characteristics, and suitability for real-time embedded speech enhancement applications.

### 🧩 Schematic Diagram
![Circuit Diagram](Circuit_Diagram.png)

### ⚙️ Hardware Implementation
![Circuit Implementation](circuit_Implementation.jpg)

# Result 
The SmartEar system successfully demonstrated real-time acoustic fault detection using Edge AI on ESP32. The MFCC feature extraction process produced clear separability between GOOD and BROKEN machine conditions, as observed in the feature explorer visualization.

The trained model achieved high-confidence classification, where prediction probabilities remained close to 1.00 for GOOD during normal operation and transitioned sharply to ~0.98–1.00 for BROKEN during faulty conditions. The prediction timeline confirmed a distinct and consistent shift from normal to abnormal behavior.

Experimental testing showed that the system reliably detects faults in real time while maintaining stability in noisy environments. The complete pipeline runs efficiently on-device, validating its suitability for low-latency, offline industrial monitoring and predictive maintenance applications.

Detailed results are available in [RESULT.md](RESULT.md)

# Future Scope
The current system can be extended to support multi-class fault classification, enabling identification of specific fault types such as bearing wear, misalignment, and imbalance. Adaptive thresholding and online learning techniques can be incorporated to improve long-term reliability under varying operating conditions.

While this implementation utilizes Edge Impulse Studio for DSP processing and model development, future work can focus on building a custom AI model and DSP pipeline from scratch, allowing greater control over optimization, feature engineering, and model performance.

Further improvements may include integration of advanced lightweight deep learning models and combining additional sensor data (e.g., vibration or temperature) for multi-modal fault detection.

Scalability can also be enhanced through edge–cloud integration, enabling centralized monitoring and analytics across multiple machines.


