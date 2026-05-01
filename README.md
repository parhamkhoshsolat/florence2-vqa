# Florence-2 Visual Question Answering

![Python](https://img.shields.io/badge/python-3.8%2B-yellow)
![PyTorch](https://img.shields.io/badge/pytorch-2.0%2B-red)
![Model](https://img.shields.io/badge/model-Florence--2-blue)

Fine-tuned Microsoft Florence-2 (771M parameters) on the Abstract Scenes subset of VQA v2.0. Upload an image, ask a question, get a contextual answer with a confidence score.

## Live Demo

[Florence-2 VQA on HuggingFace Spaces](https://huggingface.co/spaces/parhamkhoshsolat/Florence-2_VQA)

## What it does

The base Florence-2 model handles a broad set of vision-language tasks. This project narrows it: fine-tune specifically for open-ended visual question answering on cartoon-style abstract scenes, then wrap it in an interactive app.

The dataset has 150K image-question pairs (60K train / 30K validation / 60K test) covering object identification, counting, spatial relationships, and scene understanding, drawn from VQA v2.0 abstract scenes.

## Training

- Framework: PyTorch
- Optimizer: AdamW, lr=1e-5
- Epochs: 3
- Inference: beam search
- Early stopping to prevent overfitting

Full training process is in `notebooks/florence_2_finetuned_vqa_dataset.ipynb`.

## Project Structure

```
florence2-vqa/
├── app/
│   └── main.py                    # Streamlit application
├── docs/
│   ├── REPRODUCTION_GUIDE.md
│   └── Technical_report.pdf
├── models/
│   └── florence2_finetuned_model/ # Download separately
├── notebooks/
│   └── florence_2_finetuned_vqa_dataset.ipynb
├── scripts/
│   ├── download_model.py
│   └── setup.py
├── requirements.txt
└── README.md
```

## Local Setup

```bash
git clone https://github.com/Alirezakhoshsolat/florence2-vqa.git
cd florence2-vqa
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python scripts/download_model.py
streamlit run app/main.py
```

Or use the setup script which handles everything automatically:

```bash
python setup.py
```

## References

- [Florence-2 on HuggingFace](https://huggingface.co/microsoft/Florence-2-base-ft)
- [VQA v2.0 Dataset](https://visualqa.org/)

## License

MIT
