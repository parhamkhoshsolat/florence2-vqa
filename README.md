# Florence-2 Visual Question Answering

I fine-tuned a vision-language model of about 230M parameters (Florence-2-base) on 60,000 image-question pairs and shipped it live on Hugging Face Spaces.

<p>
  <a href="https://parhamkhoshsolat-florence-2-vqa.hf.space"><img src="https://img.shields.io/badge/Live_demo-Hugging_Face-5dc6ff?style=flat-square&labelColor=0a1422" alt="Live demo: Hugging Face"></a>
  <a href="https://www.parhamkhoshsolat.com/projects/florence2-vqa"><img src="https://img.shields.io/badge/Write--up-portfolio-5dc6ff?style=flat-square&labelColor=0a1422" alt="Write-up: portfolio"></a>
  <img src="https://img.shields.io/badge/2025-team_of_3-5dc6ff?style=flat-square&labelColor=0a1422" alt="2025: team of 3">
</p>

## Overview

Upload an image, ask a question in plain English, and get an answer with a model score. The base Florence-2 model covers many vision-language tasks; this project narrows it to open-ended visual question answering on the cartoon-style Abstract Scenes subset of VQA v2.0, then wraps it in a Streamlit app.

## What I did

I built the pipeline: data preparation, the fine-tune, loss tracking and the deployed app. We also tried PaliGemma (ran out of GPU memory) and BLIP (did not train stably); these runs were not logged and are not in this repo.

- Model: Florence-2-base-ft, about 230M parameters, full fine-tune with no frozen layers
- Data: 60,000 question-answer pairs (the train split; the test split ships without answers)
- Training: 3 epochs, AdamW at lr 1e-5, batch 8, linear schedule with no warmup, 22,500 steps, about 9 hours on one Colab GPU
- Inference: beam search with a per-answer score (the exponential of the best beam's length-normalised log-probability)

## Results

Only loss was measured. Validation loss on the 30,000 validation questions fell from 0.240 after epoch 1 to 0.206 and then 0.202 after epoch 3; training loss fell from 0.307 to 0.176 and 0.111. The full training process is in `notebooks/florence_2_finetuned_vqa_dataset.ipynb`.

### Limits

- **No accuracy score.** No accuracy, VQA score, exact match, BLEU or CIDEr was computed, and there is no test-set evaluation: the test split ships without answers. The validation set was scored every epoch but not used for early stopping or model choice.
- **The loss includes padding.** The answer labels are padded to the longest answer in each batch and the pad tokens are not masked out (no `-100` labels), so part of the loss is the model predicting padding. The absolute values are not comparable with other Florence-2 fine-tunes; the downward trend is real.
- **Easy answer mix.** About 40% of the training answers are "yes" or "no" (read off the answer chart in the notebook; no exact count is printed).
- **The demo's score is not a calibrated probability.** It is the exponential of the length-normalised beam log-probability, not the chance that the answer is right.
- **Shipped checkpoint not recorded.** The notebook zips the epoch-2 checkpoint folder under an epoch-3 file name and also saves the final in-memory model, so the repo does not show whether the published weights are from epoch 2 or epoch 3.

## Run it

```bash
git clone https://github.com/parhamkhoshsolat/florence2-vqa.git
cd florence2-vqa
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python scripts/download_model.py
streamlit run app/main.py
```

## Project layout

```
florence2-vqa/
├── app/main.py                     # Streamlit application
├── docs/REPRODUCTION_GUIDE.md
├── models/florence2_finetuned_model/  # downloaded by scripts/download_model.py
├── notebooks/florence_2_finetuned_vqa_dataset.ipynb
├── scripts/                        # download_model.py, setup.py
└── requirements.txt
```

## Team & course

Data Mining and Machine Learning, University of Naples Federico II. Team: Parham Khosh Solat, Zahra Jafarinejad, Farshad Farahtaj.

References: [Florence-2 on Hugging Face](https://huggingface.co/microsoft/Florence-2-base-ft) · [VQA v2.0](https://visualqa.org/)

## License

MIT. See [`LICENSE`](LICENSE).

---

Part of my portfolio: [parhamkhoshsolat.com](https://www.parhamkhoshsolat.com)
