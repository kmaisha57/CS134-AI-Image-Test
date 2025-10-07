# CS134-AI-Image-Test
Code and image set for testing the efficacy of facial image blurring methods against facial reconstruction. Completed for CS134.


This work explores the following research questions:
1. How effectively can AI models reconstruct personally identifiable information (PII) from publicly available street-level images, particularly for blurred faces?
2. What governance measures are necessary to mitigate privacy risks?

This problem is important because it speaks to the tension between data accessibility and privacy rights in the digital era. Public street imagery serves useful purposes (urban planning, mapping, accessibility, and navigation) but also risks exposing faces, homes, and behaviors (Bittner et al.; Aydın et al.). If modern generative or restoration models can effectively recreate faces from blurred regions, the assumption that these images are anonymized is undermined.

In this work, I assess actual Google Street View images and simulate Google-style blurring on a high-resolution public dataset (FFHQ), reconstruct the faces using GFP-GAN, and assess image similarity using LPIPS, SSIM, and PSNR scores. Limitations include the lack of human validation and the inability to evaluate the Street View reconstructions.

Key contributions/findings:
● Even with heavy blurring, AI models can recreate a plausible face.
● Angle, orientation, and resolution play a significant role in facial restoration.
● Image dataset collection efforts, even in the public domain, should follow some standards for enhanced trust and transparency.

# Methodology
This work evaluates the privacy risks associated with publicly blurred facial imagery. The methodology is broken into three phases: data collection and preprocessing, reconstruction using AI models, and evaluation of reconstructed images. By simulating facial blurring similar to what is used in Google Street View and applying generative restoration tools to the images, this work assesses whether blurring continues to function as an effective privacy safeguard in the era of advanced AI models.

Data Collection and Preprocessing

The dataset draws from two sources:
● Google Street View imagery, scraped using the Google Maps Static API at locations where faces were blurred in the standard Street View interface.
● A subset of the FFHQ (Flickr-Faces-HQ) dataset, a public high-resolution facial image dataset developed by NVIDIA for benchmarking generative models (Karras et al.).
For the Street View images, 50 stills were taken San Francisco, CA, New York, NY, Los Angeles, CA, Chicago, IL, Houston, TX, Miami, FL, Seattle, WA, Philadelphia, PA, Dallas, TX, and Pheonix, AZ. The images were manually assessed to identify instances of blurred faces, which were then cropped from the image. This resulted in an image set of 14 blurred faces.
The FFHQ subset consisted of 300 random images from the larger dataset of 60,000 images. The images have a resolution of 1024x1024. Because this was a set of the original, unaltered images, the efficacy of the blurring and reconstruction could be assessed.

Reconstruction Using AI Models

To mimic the type of facial blur used in Google Street View, this work used strong Gaussian blur (kernel size: 75×75, sigma: 30) to the full FFHQ images. Though real Street View blurs are face-localized, this approach assumes a more conservative scenario by applying the blur across the entire image, ensuring the reconstructed face emerges solely from degraded visual input.
This method approximates the anonymization used by Google, where faces are detected and then blurred to obscure identity. To simulate Google Street View–style privacy filters, I applied a multi-step Gaussian blur process. This method involves repeatedly applying Gaussian blur with varying kernel sizes and sigma values to incrementally degrade facial features while maintaining consistent image dimensions. Gaussian blur is a commonly used image processing technique that reduces image detail and noise. The Gaussian function assigns higher weights to the center pixels and progressively lower weights to those farther away, resulting in a smooth blurring effect that preserves general structure while eliminating fine details. For faces, it maintains the general shape of features while removing textures such as eyes, nose contours, or skin patterns. The result was a blurred image set that visually resembles real-world blurring techniques.
The reconstruction method was the same for the Google Street View images and the FFHQ images and involved two stages.
First, the blurred image was upscaled because the cropped images from Google Street View were very low-resolution. This was done using Real-ESRGAN, which upscales the blurred image by a factor of 4. The intention here is to improve the clarity and facial structure for a more successful reconstruction.
The facial reconstruction was done using GFP-GAN (Generative Facial Prior-Generative Adversarial Network), a model developed by Tencent ARC. This uses a generative adversarial network with facial priors to recover natural-looking faces from low-quality inputs. For this work, a pre-trained GFP-GANv1.3 model was used on the blurred images.
Evaluation of Reconstructed Images
This work used the three evaluation metrics. The first is SSIM (Structural Similarity Index). This assesses pixel-wise structural integrity between original and restored images from 0 (no similarity) to 1(structurally identical). Next, I measured PSNR (Peak Signal-to-Noise Ratio), which measures the absolute pixel reconstruction quality. PSNR has a wide range, but values over 30dB tend to be considered high quality reconstructions with little noise. The last one is LPIPS (Learned Perceptual Image Patch Similarity). This is a deep learning–based metric trained to capture human visual similarity. Lower LPIPS scores indicate higher perceptual similarity, so for this effort, identifying whether the reconstructed face looks similar to the original image (Zhang et al.).

Policy Recommendations
1. Declare Face Restoration Models as "High-Risk AI"
Under the EU AI Act, systems used for biometric reconstruction or facial generation could be designated as high-risk. This would subject them to transparency obligations, human oversight, and documentation of training data (European Commission).
2. Strengthen Anonymization Thresholds
Regulators (e.g. European Data Protection Board) should issue updated guidance clarifying that anonymization must account for AI reconstruction capabilities. For example, data controllers should evaluate if deep generative models could plausibly reverse any obscurement before declaring data "anonymous."
3. Require Facial Anonymization Audits
Publishers of large-scale image datasets (e.g. Google, city governments, or citizen science platforms) should be required to audit their anonymization strategies against new AI threats. This mirrors ideas from NIST’s AI Risk Management Framework, which recommends regular evaluation of emerging vulnerabilities (NIST).
4. Deletion of Unaltered Originals
Organizations implementing face blurring for privacy should also delete the original unblurred versions unless there is a compelling legal or scientific justification. Public dataset publishers should document these deletion protocols explicitly as part of their transparency commitments, following models like Google’s. Researchers and citizen scientists should adopt this standard, especially when handling images of real people in public space.
5. Public Education
Transparency is imperative in the case of facial imagery for the protection of human rights and autonomy. Thus, people should be informed of when images are being collected, by whom, how they are being processed, and how they are being kept secure. People should also be able to provide consent and easily remove their images if desired. Education and information dissemination should be a component of any facial image data collection plan.
