# GLM-OCR weights mirror

GitHub Releases mirror of the [zai-org/GLM-OCR](https://huggingface.co/zai-org/GLM-OCR) Hugging Face model snapshot. The original source code and usage instructions are at [zai-org/GLM-OCR](https://github.com/zai-org/GLM-OCR). This is an independent mirror, not an official Z.ai distribution.

This repository stores small scripts and tests; original model weights are published as numbered ZIP parts in GitHub Releases. The upstream snapshot includes model configuration, tokenizer, processor, metadata and applicable licensing or attribution files.

## Publish the model from GitHub Actions

Visit [Publish GLM-OCR weights](https://github.com/caderea/glm-ocr-weights/actions/workflows/publish-weights.yml), select **Run workflow** on `main`, and keep defaults:
- **revision**: `main`
- **tag**: `glm-ocr-v1`

The publisher resolves the Hugging Face revision to a fixed commit, streams upstream files into 1 GiB ZIP segments, uploads each part directly to a draft GitHub Release, and publishes it only after all parts and a SHA-256 manifest have uploaded.

If interrupted, a new workflow run using the same release tag and upstream revision reuses already uploaded, checksum-matching parts. It retries GitHub's initially invisible draft Releases, resumes interrupted Hugging Face HTTP downloads with Range requests, and refuses to overwrite a published release.

The workflow runs tests before downloading. If the upstream repository requires authorization, set a GitHub Actions secret named `HF_TOKEN` with permitted read access. Do not commit credentials.

## Download from another computer

Requires Git and Python 3.10+. No Hugging Face CLI, Git LFS or PyTorch is needed **just for downloading**.

```bash
git clone https://github.com/caderea/glm-ocr-weights.git
cd glm-ocr-weights

python3 scripts/weights.py download \
  --tag glm-ocr-v1 \
  --out ./models/GLM-OCR
```

This downloads GitHub Release assets, verifies each chunk, rebuilds the ZIP archive, safely extracts the original Hugging Face snapshot and verifies original file SHA-256 hashes. Make sure there is free space for the downloaded parts, temporarily assembled ZIP and restored model files.

The download command only works **after** the workflow has successfully published a Release. To run OCR inference, follow the original project's runtime and GPU requirements at https://github.com/zai-org/GLM-OCR.

## Provenance and license

Upstream model: https://huggingface.co/zai-org/GLM-OCR

Upstream source: https://github.com/zai-org/GLM-OCR

Retain all upstream license, notice and attribution files and review the model card's terms before redistributing or using the weights. The Release manifest records the immutable upstream commit and full file hashes.
