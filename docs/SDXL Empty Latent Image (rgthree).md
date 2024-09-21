
# Documentation
- Class name: SDXL Empty Latent Image (rgthree)
- Category: rgthree
- Output node: False

The SDXL Empty Latent Image (rgthree) node is used to generate a blank latent image based on specified dimensions and scaled according to the crop ratio. It is mainly used as a basic step in the image generation process to create an initial, blank latent space before further manipulation or addition.

# Input types
## Required
- dimensions
    - Specifies the size of the potential image to be generated, providing a range of predefined size options, including various aspect ratios from landscape to portrait and square. This parameter is essential for determining the basic dimensions of the potential image.
    - Comfy dtype: COMBO[STRING]
    - Python dtype: str
- clip_scale
    - Determines the scaling factor applied to the potential image size, which affects the final size of the cropped area. This parameter plays an important role in adjusting the resolution of the resulting image.
    - Comfy dtype: FLOAT
    - Python dtype: float
- batch_size
    - Controls the number of potential images generated in a single batch, allowing efficient processing of multiple images simultaneously.
    - Comfy dtype: INT
    - Python dtype: int

# Output types
- LATENT
    - Generated blank potential image.
    - Comfy dtype: LATENT
    - Python dtype: torch.Tensor
- CLIP_WIDTH
    - The width of the cropping area after applying the crop ratio to the original size.
    - Comfy dtype: INT
    - Python dtype: int
- CLIP_HEIGHT
    - Applies the crop ratio to the height of the cropped area after the original dimensions.
    - Comfy dtype: INT
    - Python dtype: int


## Usage tips
- Infra type: `GPU`
- Common nodes:
    - KSamplerAdvanced //Inspire
    - KSampler //Inspire
    - SeedExplorer //Inspire



## Source code
```python
class RgthreeSDXLEmptyLatentImage:

  NAME = get_name('SDXL Empty Latent Image')
  CATEGORY = get_category()

  @classmethod
  def INPUT_TYPES(cls):  # pylint: disable = invalid-name, missing-function-docstring
    return {
      "required": {
        "dimensions": (
          [
            # 'Custom',
            '1536 x 640   (landscape)',
            '1344 x 768   (landscape)',
            '1216 x 832   (landscape)',
            '1152 x 896   (landscape)',
            '1024 x 1024  (square)',
            ' 896 x 1152  (portrait)',
            ' 832 x 1216  (portrait)',
            ' 768 x 1344  (portrait)',
            ' 640 x 1536  (portrait)',
          ],
          {
            "default": '1024 x 1024  (square)'
          }),
        "clip_scale": ("FLOAT", {
          "default": 2.0,
          "min": 1.0,
          "max": 10.0,
          "step": .5
        }),
        "batch_size": ("INT", {
          "default": 1,
          "min": 1,
          "max": 64
        }),
      },
      # "optional": {
      #   "custom_width": ("INT", {"min": 1, "max": MAX_RESOLUTION, "step": 64}),
      #   "custom_height": ("INT", {"min": 1, "max": MAX_RESOLUTION, "step": 64}),
      # }
    }

  RETURN_TYPES = ("LATENT", "INT", "INT")
  RETURN_NAMES = ("LATENT", "CLIP_WIDTH", "CLIP_HEIGHT")
  FUNCTION = "generate"

  def generate(self, dimensions, clip_scale, batch_size):
    """Generates the latent and exposes the clip_width and clip_height"""
    if True:
      result = [x.strip() for x in dimensions.split('x')]
      width = int(result[0])
      height = int(result[1].split(' ')[0])
    latent = EmptyLatentImage().generate(width, height, batch_size)[0]
    return (
      latent,
      int(width * clip_scale),
      int(height * clip_scale),
    )

```
