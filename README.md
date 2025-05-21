Translator Models Project
This repository contains Python scripts for translating text between multiple languages using Hugging Face's transformer-based models, specifically Helsinki-NLP/opus-mt and facebook/m2m100_418M. The project demonstrates translation from English to German, English to Spanish, and Persian to Spanish or English, leveraging lightweight models suitable for various translation tasks.
Table of Contents

Overview
Installation
Usage
Supported Models
Troubleshooting
Contributing
License

Overview
The project includes examples of using two translation models:

Helsinki-NLP/opus-mt: A lightweight model for specific language pairs (e.g., English to German).
facebook/m2m100_418M: A multilingual model supporting 100 languages, including Persian, English, and Spanish.

The code supports translation of text inputs, with examples for English, Persian, and Spanish. It is designed to run in environments like Google Colab or local Python setups.
Installation

Clone the Repository:
git clone https://github.com/your-username/translator-models.git
cd translator-models


Install Dependencies:Ensure you have Python 3.8+ installed. Install the required libraries using pip:
pip install transformers torch sentencepiece


Optional: For faster inference, consider installing optimum with ONNX Runtime:
pip install optimum[onnxruntime]



Usage
The main script (translator.py or similar) demonstrates translation using two models. Below are example snippets:
Example 1: English to German (Helsinki-NLP/opus-mt)
from transformers import MarianMTModel, MarianTokenizer

model_name = "Helsinki-NLP/opus-mt-en-de"
tokenizer = MarianTokenizer.from_pretrained(model_name)
model = MarianMTModel.from_pretrained(model_name)

text = "Hello, how are you?"
inputs = tokenizer(text, return_tensors="pt", padding=True)
translated = model.generate(**inputs)
translated_text = tokenizer.decode(translated[0], skip_special_tokens=True)
print(translated_text)  # Output: Hallo, wie geht es dir?

Example 2: Persian to Spanish (facebook/m2m100_418M)
from transformers import M2M100Tokenizer, M2M100ForConditionalGeneration

model_name = "facebook/m2m100_418M"
tokenizer = M2M100Tokenizer.from_pretrained(model_name)
model = M2M100ForConditionalGeneration.from_pretrained(model_name)

tokenizer.src_lang = "fa"  # Source: Persian
tokenizer.tgt_lang = "es"  # Target: Spanish

text = "جهان جای زیبایی برای زندگی است."
inputs = tokenizer(text, return_tensors="pt", truncation=True, padding=True)
translated = model.generate(**inputs, forced_bos_token_id=tokenizer.get_lang_id("es"))
translated_text = tokenizer.decode(translated[0], skip_special_tokens=True)
print(translated_text)  # Output: El mundo es un lugar hermoso para vivir.

Notes

Ensure forced_bos_token_id matches the target language (tokenizer.tgt_lang) to avoid translation errors.
For sensitive content, verify model outputs, as safety filters may affect translations.

Supported Models

Helsinki-NLP/opus-mt:
Size: ~77 MB
Language Pairs: English-German, etc.
Use Case: Lightweight, language-pair-specific translation.


facebook/m2m100_418M:
Size: ~1.6 GB
Languages: 100 languages, including Persian (fa), Spanish (es), and English (en).
Use Case: Multilingual translation with good performance for low-resource languages.



Troubleshooting

Incorrect Translation Output: Ensure forced_bos_token_id matches the target language. Reset the model/tokenizer between translations to clear state:tokenizer = M2M100Tokenizer.from_pretrained(model_name)
model = M2M100ForConditionalGeneration.from_pretrained(model_name)


Dependencies: Verify that transformers, torch, and sentencepiece are installed.
Memory Issues: Use quantized models with optimum for lower memory usage:pip install optimum[onnxruntime]



Contributing
Contributions are welcome! Please:

Fork the repository.
Create a feature branch (git checkout -b feature/your-feature).
Commit changes (git commit -m "Add your feature").
Push to the branch (git push origin feature/your-feature).
Open a pull request.

License
This project is licensed under the MIT License. See the LICENSE file for details.
