# Kandinsky-V2

Kandinsky 2.2 This version introduces exciting enhancements, including a more powerful image encoder called CLIP and the addition of ControlNet support.

NOTE: It may take some time for the Installation on first time! ⚠️


Join [Discord](https://discord.gg/P9gGZaXWGR) for any Assist/Issues 


## Key Features

- Lightning-fast performance for seamless art creation.
- Access to advanced computational resources, including GPUs andpy  TPUs by Google Colbatory.
- Unlimited image generation for Free!
  
## First Method!
1. Access Google Colab: Visit <a href="https://colab.research.google.com/drive/15xE2n24B1UZfsIzfnNSCyh6rYr_AKWtb?usp=sharing" target="_blank">Google Colab</a> to get started with Kandinsky-V2.
2. To Ensure Proper Execution, run each Colab cell starting from the beginning.
3. In the third cell, enter the input you want to generate, and then press Enter to proceed.
4. To retrieve the captivating images generated by Kandinsky-V2, simply run the Fourth cell!


## Second Method!
1. Access Google Colab: Visit [Google Colab](https://colab.research.google.com/).
2. Create a new Colab notebook.
3. Copy this Code Given Below and Execute the Cell!


```python
!pip install git+https://github.com/ai-forever/diffusers.git
!pip install transformers
!pip install accelerate
```

4. After executing the first cell, make another cell and run the following code:

```python
import sys
from diffusers import KandinskyV22Pipeline, KandinskyV22PriorPipeline
import torch
import PIL
import torch
from diffusers.utils import load_image
from torchvision import transforms
from transformers import CLIPVisionModelWithProjection
from diffusers.models import UNet2DConditionModel
import numpy as np

DEVICE = torch.device('cuda:0')

image_encoder = CLIPVisionModelWithProjection.from_pretrained(
    'kandinsky-community/kandinsky-2-2-prior',
    subfolder='image_encoder'
).half().to(DEVICE)

unet = UNet2DConditionModel.from_pretrained(
    'kandinsky-community/kandinsky-2-2-decoder',
    subfolder='unet'
).half().to(DEVICE)

prior = KandinskyV22PriorPipeline.from_pretrained(
    'kandinsky-community/kandinsky-2-2-prior',
    image_encoder=image_encoder,
    torch_dtype=torch.float16
).to(DEVICE)

decoder = KandinskyV22Pipeline.from_pretrained(
    'kandinsky-community/kandinsky-2-2-decoder',
    unet=unet,
    torch_dtype=torch.float16
).to(DEVICE)
```


5. After sucessfully executing run this:
```python
torch.manual_seed(42)

negative_prior_prompt ='lowres, text, error, cropped, worst quality, low quality, jpeg artifacts, ugly, duplicate, morbid, mutilated, out of frame, extra fingers, mutated hands, poorly drawn hands, poorly drawn face, mutation, deformed, blurry, dehydrated, bad anatomy, bad proportions, extra limbs, cloned face, disfigured, gross proportions, malformed limbs, missing arms, missing legs, extra arms, extra legs, fused fingers, too many fingers, long neck, username, watermark, signature'
img_emb = prior(
    prompt = input("Enter a description for the image: "),
    num_inference_steps=25,
    num_images_per_prompt=1
)

negative_emb = prior(
    prompt=negative_prior_prompt,
    num_inference_steps=25,
    num_images_per_prompt=1
)

images = decoder(
    image_embeds=img_emb.image_embeds,
    negative_image_embeds=negative_emb.image_embeds,
    num_inference_steps=75,
    height=512,
    width=512)
```

4. After giving a Prompt to generate, run this to retrieve the generated Image!
```python
images.images[0] 
```

5. Enjoy the Unlimited Generations for free : )




