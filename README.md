<p align="center">
  <img src="https://raw.githubusercontent.com/PKief/vscode-material-icon-theme/main/icons/folder-ai-open.svg" width="150">
</p>

<h1 align="center">🦴 Fracture Segmentation Report</h1>

<p align="center">
  <em>A deep learning approach to identifying bone fractures in medical imagery.</em>
</p>

---
## Dataset description:
The dataset is structured in the COCO format, with image annotations stored as JSON. Each sample consists of:
- An RGB image containing fractures.
- A binary segmentation mask derived from COCO polygon annotations.
- A predefined dataset split (80% training, 20% validation).  

## Architecture:
The model is based on the Segment Anything Model (SAM) ViT-B, which includes:
- Image Encoder (Frozen) - The image encoder processes the input image into a latent feature representation using a Vision Transformer (ViT).
- Prompt Encoder (Frozen) - Encodes sparse and dense prompts.  SAM requires prompts to generate a segmentation mask.
#### These prompts can be:
-- Points (e.g., clicking on an object in the image).
-- Boxes (bounding boxes around the object).
-- Masks (partial segmentations to refine results).
The prompt encoder converts these inputs into dense embeddings that guide the segmentation process.
- Mask Decoder (Trainable) - Predicts segmentation masks from embeddings.  
We specifically train the mask decoder while keeping the encoders frozen to utilize pretrained feature extraction.  
however for producing the visulaizations before training we chose sam-h which is the larger version of sam-b but we had to exclude it since we ran into some out of memory issues and had to continue our training process with sam base (the smaller version).

## Training:
Images were resized to 1024x1024 and Converted to tensors for PyTorch processing. but Normalization is handled implicitly by the pretrained SAM model. Adam with a learning rate of 1e-4 was chosen as the optimizer and for the loss function we used Binary Cross-Entropy with Logits Loss.

## Conclusion
from the images produced by sam h we can see that the model can already very accurately detect the fraction, it has some error on finding the exact shape of the fraction but in terms of location, it could be considered state of the art, however for demostration purposes we trained the b version for two epochs(bacause of computational limitations)
the results indicate a significant low loss value. 

<img width="1182" height="323" alt="image" src="https://github.com/user-attachments/assets/a2003f32-4773-44eb-889e-5c76b51dfb11" />

<img width="1157" height="427" alt="image" src="https://github.com/user-attachments/assets/336814a7-d323-4631-a762-e364f2456343" />

<img width="1156" height="427" alt="image" src="https://github.com/user-attachments/assets/47751914-7a08-4a35-a4f5-67bd4e9293fe" />

<img width="1156" height="427" alt="image" src="https://github.com/user-attachments/assets/d6f0325d-fa57-4ead-992e-25604aae0779" />


