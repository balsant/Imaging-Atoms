# Ultracold Strontium Atom Counting in a Magneto-Optical Trap (MOT)

This code is used to calculate the number of ultracold strontium atoms in a Magneto-Optical Trap (MOT) by processing photon counts obtained from a camera. The calculation is based on various physical parameters of the system, such as laser intensity, saturation intensity, and camera properties. The code is designed to run in a loop, continuously updating the number of detected atoms.

## Code Overview

### Initialization
- **Laser and System Parameters**:
  - `I = 6.37 mW/cm^2`: Laser intensity used in the experiment (sum total for all six beams in a six-beam MOT).
  - `Is = 42.7 mW/cm^2`: Saturation intensity for the atomic transition.
  - `G = 32e6 MHz`: Natural linewidth (gamma) of the atomic transition (for 461-nm).
  - `D = 40e6 MHz`: Detuning of the laser from the atomic resonance.
  - `T = 1.96e-7 s`: Camera exposure time, which can be retrieved dynamically using the `GetExposureTime()` function.
  
- **Optical Setup**:
  - `r = 7.5 mm`: Radius of the camera lens (half of the lens diameter).
  - `R = 160 mm`: Distance from the MOT to the camera lens (equals 2 times the focal length, where `f = 80 mm`).

### Photon Detection Calculation
- **Photon Detection Fraction**:
  - `fr = (r^2) / (4 * R^2)`: Fraction of the total photons emitted by the MOT that are captured by the camera lens.
  
- **Solid Angle Factor**:
  - `Solid = 4 * (R^2) / (r^2)`: A factor (inverse of `fr`) used later to calculate the number of atoms based on the geometry of the imaging system.

### Photon Scattering Rate
- **Photon Scattering Rate per Atom**:
  - `P = 0.5 * G * 2 * 3.14 * (I / Is) / (1 + I / Is + (4 * D^2) / (G^2))`: Calculates the photon scattering rate per atom based on the laser intensity, detuning, and linewidth.

### Data Collection Loop
- The code enters a loop to repeatedly collect and process photon counts:
  - **Photon Count Acquisition**:
    - `ph = TotalCounts(#0)`: Retrieves the total photon counts from the camera.
  
  - **Corrections Applied**:
    - `ph = ph * 0.45`: Corrects for the camera's sensitivity in 16-bit mode.
    - `ph = ph / 0.62`: Corrects for the camera's quantum efficiency (QE).
    - `ph = ph / 0.89`: Corrects for optical attenuation (filter, lens, window).

  - **Normalization and Atom Number Calculation**:
    - `ph = ph / ex`: Normalizes the photon count by exposure time to get the photon rate.
    - `N = ph * Solid / P`: Calculates the number of atoms based on the photon rate, solid angle factor, and photon scattering rate.
  
  - **Data Storage**:
    - The calculated atom number `N` is stored in a data structure for further analysis or display.

  - **Scaling and Display**:
    - The data is scaled and printed for monitoring purposes. The loop continues until a counter reaches 100, after which it resets.

### Output and Monitoring
- The code continuously prints the time, a scaling factor, and the calculated number of atoms, allowing real-time monitoring of the MOT's atom count.

## Usage
1. **Set up the experiment** and adjust the parameters (`I`, `Is`, `G`, `D`, `T`, `r`, `R`) according to your specific setup.
2. **Run the code** to start collecting and processing photon counts.
3. **Monitor the output** to see the real-time atom count and adjust your experimental parameters as necessary.

## Dependencies
- This code assumes the use of specific camera (Andor Zyla) and data acquisition functions (`TotalCounts`, `GetExposureTime`, `create`, `run`, `ScaleData`, `update`) in Andor Solis software.

