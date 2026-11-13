# Florence-2 Visual Question Answering

I fine-tuned a 230M-parameter vision-language model on 60,000 image-question pairs and shipped it live on Hugging Face Spaces.

<p>
  <a href="https://parhamkhoshsolat-florence-2-vqa.hf.space"><img src="https://img.shields.io/badge/Live_demo-Hugging_Face-5dc6ff?style=flat-square&labelColor=0a1422" alt="Live demo: Hugging Face"></a>
  <a href="https://www.parhamkhoshsolat.com/projects/florence2-vqa"><img src="https://img.shields.io/badge/Write--up-portfolio-5dc6ff?style=flat-square&labelColor=0a1422" alt="Write-up: portfolio"></a>
  <img src="https://img.shields.io/badge/2025-team_of_3-5dc6ff?style=flat-square&labelColor=0a1422" alt="2025: team of 3">
</p>

## Overview

Upload an image, ask a question in plain English, and get an answer with a confidence score. The base Florence-2 model covers many vision-language tasks; this project narrows it to open-ended visual question answering on the cartoon-style Abstract Scenes subset of VQA v2.0, then wraps it in a Streamlit app.

## What I did

I built the full pipeline: data preparation, the fine-tune, evaluation and the deployed app. PaliGemma 3B ran out of GPU memory and BLIP did not train stably, so Florence-2 was kept.

- Model: Florence-2-base-ft, about 230M parameters, full fine-tune with no frozen layers
- Data: 60,000 question-answer pairs (the train split; the test split ships without answers)
- Training: 3 epochs, AdamW at lr 1e-5, batch 8, on Colab
- Inference: beam search with a per-answer confidence score

## Results

Final validation loss 0.202. No accuracy metric was computed; answers were checked by hand. The full training process is in `notebooks/florence_2_finetuned_vqa_dataset.ipynb`.

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
