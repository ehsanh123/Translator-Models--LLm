# Multilingual Translator with Hugging Face Transformers

This project demonstrates how to build simple multilingual translation pipelines using Hugging Face’s [`transformers`](https://huggingface.co/docs/transformers/index) library. It features translation from English to German and Spanish using OPUS-MT and M2M100 models, and includes basic exploration of issues with context persistence and language directionality.

## 🧠 Models Used
- [`Helsinki-NLP/opus-mt-en-de`](https://huggingface.co/Helsinki-NLP/opus-mt-en-de): English ➝ German translation
- [`facebook/m2m100_418M`](https://huggingface.co/facebook/m2m100_418M): Multilingual (e.g., English ↔ Spanish, Persian ↔ Spanish, Persian ↔ English)

## 📊 Features
- English to German translation using MarianMT
- English to Spanish translation using M2M100
- Persian to Spanish & Persian to English translation
- Discussion of translation errors caused by model state retention or incorrect token forcing

## 🛠️ How It Works

### 1. English ➝ German (MarianMT)
```python
from transformers import MarianMTModel, MarianTokenizer

model_name = "Helsinki-NLP/opus-mt-en-de"
tokenizer = MarianTokenizer.from_pretrained(model_name)
model = MarianMTModel.from_pretrained(model_name)

text = "Hello, how are you?"
inputs = tokenizer(text, return_tensors="pt", padding=True)
translated = model.generate(**inputs)
print(tokenizer.decode(translated[0], skip_special_tokens=True))
