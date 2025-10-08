# sCT Generation from Nuclear Medicine Scans: A Patch-Based Ensemble Approach

This project showcases an advanced deep learning pipeline designed to address a critical challenge in medical imaging: generating high-fidelity **synthetic CT (sCT) images** directly from nuclear medicine scans like PET. By providing rich anatomical context where it is typically absent, this technology enhances the diagnostic value of functional scans and opens new possibilities for treatment planning and analysis.

Our approach, built on PyTorch Lightning, leverages a full-resolution, patch-based training strategy with a novel **ensemble of five state-of-the-art models**, ensuring both computational efficiency and exceptional anatomical accuracy.

## The Challenge: Adding Anatomy to Function

Nuclear medicine scans are powerful for revealing metabolic function, but they lack the detailed anatomical information of a CT scan. This project bridges that gap by training a generative model to translate a nuclear medicine image into a corresponding CT image. To ensure the generated CT is not just plausible but also anatomically precise, we've developed a unique, dual-model loss mechanism.

## Key Innovations & Expertise

This project demonstrates our expertise in building sophisticated, reliable, and efficient generative models for medical imaging.

### 1. The Ensemble of Experts
Instead of a single monolithic model, our pipeline uses an **ensemble of five independent nnU-Net models**, each pretrained as an "expert" on a specific anatomical region using the TotalSegmentator dataset (organs, vertebrae, cardiac, muscles, and ribs). During training, these experts work in concert, each providing its specialized anatomical knowledge to a set of lightweight "adapter" networks that generate the final sCT. This division of labor ensures a higher level of detail and accuracy than a single model could achieve alone.

### 2. The "Anatomical Supervisor": A Dual-Model Loss
A key innovation is our method for ensuring the generated sCT is anatomically correct. We use a parallel, identical ensemble of segmentation models whose weights are **frozen** during training. This "anatomical supervisor" constantly evaluates the synthetic CT produced by the main model. If the generated sCT "confuses" the supervisor, a strong loss signal is sent back, forcing the generator to produce images that are not only visually realistic but also make perfect anatomical sense to another expert AI. This self-correction mechanism is critical for building trust in the generated images.

### 3. Efficient Patch-Based Training
To handle high-resolution medical images efficiently, the entire pipeline is built on a **patch-based training loop**. By feeding the model smaller `128x128x128` patches, we can train on full-resolution data without the prohibitive memory requirements of full-image training, making the development of such advanced models more feasible.

## Clinical Impact & Significance

This work represents a significant step forward in computational medical imaging. By generating high-quality synthetic CTs, this technology has the potential to:
- **Enhance Diagnostic Confidence** by providing crucial anatomical context for functional scans.
- **Improve Attenuation Correction** in PET imaging, leading to more accurate quantification.
- **Enable More Precise Treatment Planning** by allowing clinicians to better localize tumors and other pathologies.

This project showcases our ability to design and implement complex, multi-model AI systems that solve real-world clinical challenges.

[Back to all projects](../README.md)