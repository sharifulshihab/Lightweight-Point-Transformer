# Lightweight Point Transformer for 3D Object Part Segmentation

A lightweight implementation of the Point Transformer for 3D object part segmentation, focusing on reducing model parameters while retaining the original architecture's key components.

## Project Overview

This project explores parameter reduction in the Point Transformer architecture for 3D object part segmentation.

The lightweight modifications focus on:

- **Bottleneck reduction:** Reducing the internal feature dimensions within transformer blocks.
- **Class embedding reduction:** Reducing the shape-class embedding dimension from 1024 to 256.

The original five-level encoder-decoder structure and vector attention mechanism are retained.

## Model Variants

The implementation provides three lightweight model variants:

- `PointTransformer-PartSeg26-Light`
- `PointTransformer-PartSeg38-Light`
- `PointTransformer-PartSeg50-Light`

**Experimental note:** The reported training and evaluation experiment used `PointTransformer-PartSeg38-Light`.

### Implementation Details

The reported PartSeg38-Light experiment uses:

- `bottleneck_ratio=2`
- `mid_ratio=1`
- `cls_dim=256`

The implementation adapts the Point Transformer V1 part-segmentation model by reducing the internal transformer-block width and shape-class embedding dimension while retaining its core encoder-decoder structure and vector attention mechanism.

## Experimental Environment

The lightweight model was implemented and trained using the following environment:

| Component | Configuration |
|---|---|
| Framework | Pointcept |
| Deep learning framework | PyTorch 2.11.0 |
| CUDA | 12.8 |
| GPU | NVIDIA GeForce RTX 5090 |
| GPU Memory | 32 GB |
| Dataset | ShapeNetPart |

The model was trained from scratch without pretrained weights.

## Dataset

Experiments were conducted using ShapeNetPart.

- 16 object categories
- 50 part classes
- 14,006 training and validation shapes
- 2,874 test shapes
- 6 input channels: XYZ coordinates and normals
- 2,048 points per shape

## Training Configuration

| Setting | Value |
|---|---|
| Optimizer | AdamW |
| Weight decay | 0.02 |
| Maximum learning rate | 0.001 |
| Learning-rate scheduler | OneCycleLR |
| Warm-up | 5% |
| Initial / final learning rate | 0.0001 / 0.0000001 |
| Loss function | Cross-entropy |
| Batch size | 16 shapes |
| Training length | 200 passes (100 scheduled epochs, loop = 2) |
| Mixed precision | Off |
| Gradient clipping | None |
| Data augmentation | Off |

## Reported Results

The trained `PointTransformer-PartSeg38-Light` model contains **3.264 million parameters**.

| Model | Parameters |
|---|---:|
| Published Point Transformer PartSeg50 | 8.311 M |
| Trained PartSeg38-Light | 3.264 M |

This represents an approximately **60.7% reduction in parameter count** compared with the published Point Transformer PartSeg50 model.

On the ShapeNetPart benchmark, PartSeg38-Light achieved:

- **Class mIoU:** 83.05%
- **Instance mIoU:** 85.50%

**Comparison note:** The baseline parameter count and performance metrics are taken from the published Point Transformer paper. The original model was not retrained using the same experimental configuration as PartSeg38-Light. Therefore, the reported comparison does not represent a controlled evaluation under identical training conditions.

## Limitations and Future Work

### Current Limitations

- Only `PointTransformer-PartSeg38-Light` was trained and evaluated.
- PartSeg26-Light and PartSeg50-Light are included as model configurations, but their segmentation performance has not been evaluated.
- Results are based on a single training run.
- The original Point Transformer baseline was not retrained under the same experimental settings.
- No ablation study was conducted to isolate the effect of each architectural change.
- Training time, inference latency, and memory usage were not measured.

### Future Work

- Train and evaluate the 26- and 50-layer lightweight variants.
- Retrain the original baseline under identical experimental conditions.
- Conduct multiple runs and ablation studies.
- Measure inference latency, memory usage, and training time.
- Explore knowledge distillation, pruning, and quantization.
- Evaluate on additional point-cloud benchmarks.

## Dependencies

The model relies on:

- Python
- PyTorch
- einops
- Pointcept framework
- Pointcept's `pointops` operations
- Pointcept's `LayerNorm1d` utility

**Note:** `pointops` is a CUDA-based extension, and the model depends on the Pointcept framework. These components require the appropriate framework setup rather than being treated as ordinary standalone Python packages.

## Pointcept Dependency and Integration

This implementation is designed to work within the [Pointcept](https://github.com/Pointcept/Pointcept) framework.

### Integration

To use the modified model:

1. Clone and configure the [Pointcept repository](https://github.com/Pointcept/Pointcept).
2. Place `point_transformer_partseg_lightweight.py` inside Pointcept's `pointcept/models/point_transformer/` directory.
3. Import the new model classes in that directory's `__init__.py`.
4. Select the desired lightweight model variant in the Pointcept configuration.

**Note:** This repository contains the modified model implementation, not the complete Pointcept framework or compiled PointOps extension.

The original model weights are not directly compatible with the reduced dimensions. The lightweight variants must be trained from scratch.

## Acknowledgements

This project adapts the Point Transformer V1 part-segmentation implementation and uses the Pointcept framework for model integration and point-cloud processing.

We acknowledge the original authors and contributors whose work provided the foundation for this implementation.

- **Framework:** [Pointcept](https://github.com/Pointcept/Pointcept)
- **Upstream model implementation:** [Point Transformer V1 Part Segmentation](https://github.com/Pointcept/PointTransformerV2/blob/main/pcr/models/point_transformer/point_transformer_partseg.py)
- **Upstream repository:** [PointTransformerV2](https://github.com/Pointcept/PointTransformerV2)