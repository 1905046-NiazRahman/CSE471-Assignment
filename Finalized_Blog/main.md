# Visual AutoRegressive Modeling (VAR): A New Era for Autoregressive Models in Computer Vision

The field of computer vision has been revolutionized by advancements in deep learning, particularly in the area of image generation. Generative Adversarial Networks (GANs) and diffusion models have dominated the landscape, demonstrating remarkable capabilities in synthesizing realistic images. However, another contender, **autoregressive (AR) models**, inspired by the success of large language models (LLMs) like GPT, has been steadily emerging. 

While AR models have shown promise in image generation, they have been plagued by limitations that have prevented them from reaching the performance levels of their GAN and diffusion-based counterparts. A paper presented at NeurIPS 2024, "Visual Autoregressive Modeling: Scalable Image Generation via Next-Scale Prediction"[^1], introduces **VAR**, a novel framework that addresses these limitations and unlocks the true potential of AR models in computer vision.

![Figure 1: Comparison of two standard autoregressive modeling (AR) models and Visual AutoRegressive modeling (VAR) model](1.png)

**Figure 1**: Comparison of two standard autoregressive modeling (AR) models and Visual AutoRegressive modeling (VAR) model:
**(a)** AR applied to language: sequential text token generation from left to right, word by word (e.g., GPT, LaMDA, PaLM).
**(b)** AR applied to images: sequential visual token generation in a raster-scan order, from left to right, top to bottom (e.g., iGPT, VQGAN, Parti).
**(c)** VAR for images: multi-scale token maps are autoregressively generated from coarse to fine scales (lower to higher resolutions), with parallel token generation within each scale. A multi-scale VQVAE is necessary for VAR to function. (taken from [^1])

**Table 1**: Generative model family comparison on class-conditional ImageNet 256×256. **↓** and **↑** indicate lower or higher values are better. Metrics include Fréchet inception distance (FID), inception score (IS), precision (Pre), and recall (Rec). Lower FID values and higher IS, Pre, Rec values are better. `#Para`: number of parameters, `#Step`: number of model runs needed to generate an image, †: taken from MaskGIT [^2]. Wall-clock inference time relative to VAR is reported. Models with the suffix ''-re'' used rejection sampling. Table taken from [^1].

| **Type** | **Model**      | **FID ↓** | **IS ↑** | **Pre ↑** | **Rec ↑** | **#Para** | **#Step** | **Time**  |
|----------|----------------|-----------|----------|-----------|-----------|-----------|-----------|-----------|
| **GAN**  | BigGAN [^3]    | 6.95      | 224.5    | **0.89**      | 0.38      | 112M      | 1         | -         |
| **GAN**  | GigaGAN [^4]   | 3.45      | 225.5    | 0.84      | **0.61**      | 569M      | 1         | -         |
| **GAN**  | StyleGAN-XL [^5]| 2.30     | 265.1    | 0.78      | 0.53      | 166M      | 3         | 0.3 [^5]  |
| **Diff.**| ADM [^6]       | 10.94     | 101.0    | 0.69      | 0.63      | 554M      | 250       | 168 [^5]  |
| **Diff.**| CDM [^7]       | 4.88      | 158.7    | -         | -         | 8100M     | -         | -         |
| **Diff.**| LDM-4-G [^8]   | 3.60      | 247.7    | -         | -         | 400M      | 250       | -         |
| **Diff.**| DiT-L/2 [^9]   | 5.02      | 167.2    | 0.75      | 0.57      | 458M      | 250       | 31        |
| **Diff.**| DiT-XL/2 [^9]  | 2.27      | 278.2    | 0.83      | 0.57      | 675M      | 250       | 45        |
| **Diff.**| L-DiT-3B [^10]   | 2.10      | 304.4    | 0.82      | 0.60      | 3.0B      | 250       | >45       |
| **Diff.**| L-DiT-7B [^10]   | 2.28      | 316.2    | 0.83      | 0.58      | 7.0B      | 250       | >45       |
| **Mask.**| MaskGIT [^2]   | 6.18      | 182.1    | 0.80      | 0.51      | 227M      | 8         | 0.5 [^2]  |
| **Mask.**| RCG (cond.) [^11]| 3.49     | 215.5    | -         | -         | 502M      | 20        | 1.9 [^11]  |
| **AR**   | VQVAE-2<sup>†</sup> [^12]  | 31.11     | ~45      | 0.36      | 0.57      | 13.5B     | 5120      | -         |
| **AR**   | VQGAN<sup>†</sup>  [^13]    | 18.65     | 80.4     | 0.78      | 0.26      | 227M      | 256       | 19 [^2]   |
| **AR**   | VQGAN [^13]     | 15.78     | 74.3     | -         | -         | 1.4B      | 256       | 24        |
| **AR**   | VQGAN-re [^13]  | 5.20      | 280.3    | -         | -         | 1.4B      | 256       | 24        |
| **AR**   | ViTVQ [^14]     | 4.17      | 175.1    | -         | -         | 1.7B      | 1024      | >24       |
| **AR**   | ViTVQ-re [^14]  | 3.04      | 227.4    | -         | -         | 1.7B      | 1024      | >24       |
| **AR**   | RQTran. [^15]   | 7.55      | 134.0    | -         | -         | 3.8B      | 68        | 21        |
| **VAR**  | VAR-d16        | 3.30      | 274.4    | 0.84      | 0.51      | 310M      | 10        | 0.4       |
| **VAR**  | VAR-d20        | 2.57      | 302.6    | 0.83      | 0.56      | 600M      | 10        | 0.5       |
| **VAR**  | VAR-d24        | 2.09      | 312.9    | 0.82      | 0.59      | 1.0B      | 10        | 0.6       |
| **VAR**  | VAR-d30        | 1.92      | 323.1    | 0.82      | 0.59      | 2.0B      | 10        | 1         |
| **VAR**  | VAR-d30-re     | **1.73**      | **350.2**    | 0.82      | 0.60      | 2.0B      | 10        | 1         |
|          | (validation data) | 1.78   | 236.9    | 0.75      | 0.67      | -         | -         | -         |



**Table 2**: Generative model family comparison on class-conditional ImageNet 512x512. **↓** and **↑** indicate lower or higher values are better. Metrics include Fréchet inception distance (FID), inception score (IS), precision (Pre), and recall (Rec). Lower FID values and higher IS values are better. ''-s'': a single shared AdaLN layer is used due to resource limitation. Table taken from [^1].

|**Type** | **Model** | **FID ↓** | **IS ↑** | **Time** |
|-|-|-|-|-|
| **GAN** | BigGAN [^3]| 8.43| 177.9| − |
|**Diff.** | ADM [^6] | 23.24 | 101.0 | − |
|**Diff.**| DiT-XL/2 [^9]| 3.04| 240.8| 81|
|**Mask.**| MaskGIT [^2]| 7.32| 156.0| 0.5<sup>†</sup>|
|**AR**| VQGAN [^13]| 26.52| 66.8| 25<sup>†</sup>|
|**VAR**| VAR-d36-s | **2.63** | **303.2** | 1|

## References
[^1]: Keyu Tian, Yi Jiang, Zehuan Yuan, BINGYUE PENG, & Liwei Wang (2024). Visual Autoregressive Modeling: Scalable Image Generation via Next-Scale Prediction. In The Thirty-eighth Annual Conference on Neural Information Processing Systems.

[^2]: H. Chang, H. Zhang, L. Jiang, C. Liu, and W. T. Freeman. Maskgit: Masked generative image transformer. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pages 11315–
11325, 2022.

[^3]: A. Brock, J. Donahue, and K. Simonyan. Large scale gan training for high fidelity natural image synthesis.
arXiv preprint arXiv:1809.11096, 2018.

[^4]: M. Kang, J.-Y. Zhu, R. Zhang, J. Park, E. Shechtman, S. Paris, and T. Park. Scaling up gans for text-to-
image synthesis. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition,
pages 10124–10134, 2023.

[^5]: A. Sauer, K. Schwarz, and A. Geiger. Stylegan-xl: Scaling stylegan to large diverse datasets. In ACM
SIGGRAPH 2022 conference proceedings, pages 1–10, 2022.

[^6]: P. Dhariwal and A. Nichol. Diffusion models beat gans on image synthesis. Advances in neural information
processing systems, 34:8780–8794, 2021.

[^7]: J. Ho, C. Saharia, W. Chan, D. J. Fleet, M. Norouzi, and T. Salimans. Cascaded diffusion models for high
fidelity image generation. The Journal of Machine Learning Research, 23(1):2249–2281, 2022. 

[^8]: R. Rombach, A. Blattmann, D. Lorenz, P. Esser, and B. Ommer. High-resolution image synthesis with
latent diffusion models. In Proceedings of the IEEE/CVF conference on computer vision and pattern
recognition, pages 10684–10695, 2022.

[^9]: W. Peebles and S. Xie. Scalable diffusion models with transformers. In Proceedings of the IEEE/CVF
International Conference on Computer Vision, pages 4195–4205, 2023.

[^10]: Alpha-VLLM. Large-dit-imagenet. https://github.com/Alpha-VLLM/LLaMA2-Accessory/tree/
f7fe19834b23e38f333403b91bb0330afe19f79e/Large-DiT-ImageNet, 2024.

[^11]: T. Li, D. Katabi, and K. He. Self-conditioned image generation via generating representations. arXiv
preprint arXiv:2312.03701, 2023.

[^12]: A. Razavi, A. Van den Oord, and O. Vinyals. Generating diverse high-fidelity images with vq-vae-2.
Advances in neural information processing systems, 32, 2019.

[^13]:  P. Esser, R. Rombach, and B. Ommer. Taming transformers for high-resolution image synthesis. In
Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, pages 12873–12883, 2021. 

[^14]: J. Yu, X. Li, J. Y. Koh, H. Zhang, R. Pang, J. Qin, A. Ku, Y. Xu, J. Baldridge, and Y. Wu. Vector-quantized
image modeling with improved vqgan. arXiv preprint arXiv:2110.04627, 2021. 

[^15]: D. Lee, C. Kim, S. Kim, M. Cho, and W.-S. Han. Autoregressive image generation using residual
quantization. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition,
pages 11523–11532, 2022. 

[^16]:

[^17]: