# Documentation
- Class name: AbsCosWave
- Category: FizzNodes 📅🅕🅝/WaveNodes
- Output node: False
- Repo Ref: https://github.com/FizzleDorf/ComfyUI_FizzNodes

The node generates waveform mode based on a set of input parameters to simulate oscillating behavior with adjustable characteristics.

# Input types
## Required
- phase
    - The phase parameter determines the interval between the wave oscillating and affects the frequency and cycle of the wave mode.
    - Comfy dtype: FLOAT
    - Python dtype: float
- amplitude
    - The size of the amplitude control of the waves determines the peak and valley value in the wave mode.
    - Comfy dtype: FLOAT
    - Python dtype: float
- x_translation
    - X_translation Move the waves along the X -axis level to change the position of the wave mode without changing its shape.
    - Comfy dtype: FLOAT
    - Python dtype: float
- max_value
    - The maximum value parameter sets the upper limit of the waves to ensure that the output remains within the definition range.
    - Comfy dtype: FLOAT
    - Python dtype: float
- current_frame
    - The current frame represents the progress of waves with time, and each frame corresponds to one point in the wave cycle.
    - Comfy dtype: INT
    - Python dtype: int

# Output types
- output
    - The output represents the calculation value of the current frame waves, which reflects the processing of the input parameter.
    - Comfy dtype: FLOAT
    - Python dtype: float
- int_output
    - The integer output is the four -house five -in -five value of the wave computing output, which provides discrete representations of the wave mode.
    - Comfy dtype: INT
    - Python dtype: int

# Usage tips
- Infra type: CPU

# Source code
```
class AbsCosWave:

    @classmethod
    def INPUT_TYPES(s):
        return {'required': {'phase': ('FLOAT', {'default': 1.0, 'min': 0.0, 'max': 9999.0, 'step': 1.0}), 'amplitude': ('FLOAT', {'default': 0.5, 'min': 0.0, 'max': 9999.0, 'step': 0.1}), 'x_translation': ('FLOAT', {'default': 0.0, 'min': 0.0, 'max': 9999.0, 'step': 1.0}), 'max_value': ('FLOAT', {'default': 0.5, 'min': 0.0, 'max': 9999.0, 'step': 0.05}), 'current_frame': ('INT', {'default': 1.0, 'min': 0.0, 'max': 9999.0, 'step': 1.0})}}
    RETURN_TYPES = ('FLOAT', 'INT')
    FUNCTION = 'Wave'
    CATEGORY = 'FizzNodes 📅🅕🅝/WaveNodes'

    def Wave(self, phase, amplitude, x_translation, max_value, current_frame):
        output = max_value - np.abs(np.cos(current_frame / phase)) * amplitude
        print(output)
        return (output, int(output))
```