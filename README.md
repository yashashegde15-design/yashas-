# BrainCare AI — best-effort PyTorch checkpoint demo

This version is set up to run using only the uploaded `tumor_cnn.pth` checkpoint, without requiring the original training notebook. It reconstructs a plausible model layout from parameter names and tensor shapes.

## What was verified from the checkpoint

The checkpoint loads as a PyTorch state dictionary. Tensor shapes show:
- `features.0.weight`: `(16, 3, 3, 3)` — first convolution accepts 3 channels and produces 16 channels.
- `features.3.weight`: `(32, 16, 3, 3)` — second convolution produces 32 channels.
- `classifier.1.weight`: `(64, 2048)` — first dense layer accepts 2048 features and produces 64.
- `classifier.3.weight`: `(2, 64)` — final dense layer produces two outputs.

## Important limitations

The original training architecture, preprocessing, output interpretation and class-label order are not included in the weights. `model_definition.py` is a best-effort reconstruction; matching parameter shapes is not proof that the original architecture is identical.

This app uses experimental assumptions to demonstrate inference:
- RGB images resized to 32 x 32;
- pixel scaling from 0–255 to 0–1;
- the final two values are treated as logits;
- labels are displayed as `Class 0 (unmapped)` and `Class 1 (unmapped)`.

**The app intentionally does not call either class “Tumor” or “No Tumor.”** That mapping cannot be inferred from the weights alone. To create a tumor/no-tumor display, the label mapping must be learned from trustworthy training information or verified using appropriately labeled data. Do not use this demo for clinical decisions.

## Run in Google Colab

1. Upload/extract this project folder in Colab.
2. Install libraries:

   ```bash
   pip install -r requirements.txt
   ```

3. To run Streamlit inside Colab, a tunnel or hosted deployment workflow is required; easier for the final public demo is to upload the files to GitHub and deploy on Streamlit Community Cloud.
4. For GitHub deployment, upload `app.py`, `model_definition.py`, `requirements.txt`, and `tumor_cnn.pth` to the same repository. Do not upload patient MRI images.

## Medical safety

Educational prototype only. It is not a clinically validated diagnostic system and must not be used to diagnose or rule out a brain tumor. MRI studies require qualified professional interpretation.
