# Documentation
- Class name: Image3D
- Category: ♾️Mixlab/Image
- Output node: True
- Repo Ref: https://github.com/shadowcz007/comfyui-mixlab-nodes.git

The Image3D node is designed to process and manipulate 3D images. It accepts a base64 encoded image and optional materials as input, converting them into a tensor form suitable for further processing by deep learning models. The node also handles mask and background image extraction, providing a comprehensive set of image manipulation capabilities。

# Input types
## Required
- upload
    - The 'upload' parameter is critical to the node as it contains base64 encoded image data and optional material for processing. It is critical to the execution of the node as it provides the primary input for image manipulation.
    - Comfy dtype: Dict[str, str]
    - Python dtype: Dict[str, Union[str, torch.]
- material
    - The 'material' parameter is optional, allowing the inclusion of additional image data that can be used to enhance the main image processing process. It adds flexibility to the node functionality by enabling the use of supplementary visual elements.
    - Comfy dtype: IMAGE
    - Python dtype: Optional[torch.]

# Output types
- IMAGE
    - 'IMAGE' outputs represent 3D images processed in tensor form and can be used for downstream tasks such as machine learning or computer vision applications.
    - Comfy dtype: IMAGE
    - Python dtype: torch.
- MASK
    - The 'MASK' output provides a binary mask derived from the input image and can be used for segmentation or other image analysis tasks.
    - Comfy dtype: IMAGE
    - Python dtype: torch.Tensor
- BG_IMAGE
    - The 'BG_IMAGE' output is an optional background image that complements the main image, enhancing the context for certain applications.
    - Comfy dtype: IMAGE
    - Python dtype: Optional[torch.Tensor]
- MATERIAL
    - The 'MATERIAL' output is a processed material image that can be used with the main image for more complex image processing tasks.
    - Comfy dtype: IMAGE
    - Python dtype: Optional[torch.Tensor]

# Usage tips
- Infra type: CPU

# Source code
```
class Image3D:

    @classmethod
    def INPUT_TYPES(s):
        return {'required': {'upload': ('THREED',)}, 'optional': {'material': ('IMAGE',)}}
    RETURN_TYPES = ('IMAGE', 'MASK', 'IMAGE', 'IMAGE')
    RETURN_NAMES = ('IMAGE', 'MASK', 'BG_IMAGE', 'MATERIAL')
    FUNCTION = 'run'
    CATEGORY = '♾️Mixlab/Image'
    INPUT_IS_LIST = False
    OUTPUT_IS_LIST = (False, False, False, False)
    OUTPUT_NODE = True

    def run(self, upload, material=None):
        image = base64_to_image(upload['image'])
        mat = None
        if 'material' in upload and upload['material']:
            mat = base64_to_image(upload['material'])
            mat = mat.convert('RGB')
            mat = pil2tensor(mat)
        mask = image.split()[3]
        image = image.convert('RGB')
        mask = mask.convert('L')
        bg_image = None
        if 'bg_image' in upload and upload['bg_image']:
            bg_image = base64_to_image(upload['bg_image'])
            bg_image = bg_image.convert('RGB')
            bg_image = pil2tensor(bg_image)
        mask = pil2tensor(mask)
        image = pil2tensor(image)
        m = []
        if not material is None:
            m = create_temp_file(material[0])
        return {'ui': {'material': m}, 'result': (image, mask, bg_image, mat)}
```