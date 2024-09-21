# Documentation
- Class name: AbsSinWave
- Category: FizzNodes 📅🅕🅝/WaveNodes
- Output node: False
- Repo Ref: https://github.com/FizzleDorf/ComfyUI_FizzNodes

Generate a absolute sine wave mode, which oscillates between the specified maximum value and zero, which is affected by the phase, amplitude and conversion parameters.This node aims to provide multi -function waveforms for various applications such as signal processing or animation.

# Input types
## Required
- phase
    - The phase parameter determines the cycle of sine waves, affecting the frequency and overall mode of the waves.This is essential for adjusting the time of oscillation in the waveform.
    - Comfy dtype: FLOAT
    - Python dtype: float
- amplitude
    - The amplitude sets the height of a sine wave to control the amplitude of the oscillation.It is the basic parameter of defining waveform strength.
    - Comfy dtype: FLOAT
    - Python dtype: float
- x_translation
    - x_translation The parameters move the waveform along the X -axis, allowing horizontal movement in the waveform mode.It is very important for positioning waveforms in the given context.
    - Comfy dtype: FLOAT
    - Python dtype: float
- max_value
    - max_value The upper limit of the waves is established to define the maximum point that sine waves can reach.It is a key parameter for setting wave oscillating scale.
    - Comfy dtype: FLOAT
    - Python dtype: float
- current_frame
    - current_frame Parameters represent the current position in time or sequence. The wave function uses it to calculate its output.It is essential for the correct waveform at a certain time.
    - Comfy dtype: INT
    - Python dtype: int

# Output types
- output
    - The output provides the calculation value of the absolute sine wave of the current frame, which represents the Y coordinate of the waveform.
    - Comfy dtype: FLOAT
    - Python dtype: float
- int_output
    - int_output It is an integer output, which is very useful for applications that require discrete values ​​rather than continuity.
    - Comfy dtype: INT
    - Python dtype: int

# Usage tips
- Infra type: CPU

# Source code
```
class AbsSinWave:

    @classmethod
    def INPUT_TYPES(s):
        return {'required': {'phase': ('FLOAT', {'default': 1.0, 'min': 0.0, 'max': 9999.0, 'step': 1.0}), 'amplitude': ('FLOAT', {'default': 0.5, 'min': 0.0, 'max': 9999.0, 'step': 0.1}), 'x_translation': ('FLOAT', {'default': 0.0, 'min': 0.0, 'max': 9999.0, 'step': 1.0}), 'max_value': ('FLOAT', {'default': 0.5, 'min': 0.0, 'max': 9999.0, 'step': 0.05}), 'current_frame': ('INT', {'default': 1.0, 'min': 0.0, 'max': 9999.0, 'step': 1.0})}}
    RETURN_TYPES = ('FLOAT', 'INT')
    FUNCTION = 'Wave'
    CATEGORY = 'FizzNodes 📅🅕🅝/WaveNodes'

    def Wave(self, phase, amplitude, x_translation, max_value, current_frame):
        output = max_value - np.abs(np.sin(current_frame / phase)) * amplitude
        print(output)
        return (output, int(output))
```