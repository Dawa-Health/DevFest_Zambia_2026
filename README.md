# Multilingual Maternal Health Assistant

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_USERNAME/YOUR_REPO/blob/main/notebooks/multilingual_maternal_health_assistant.ipynb)

A Google Colab notebook that combines language detection, translation and a maternal-health chatbot for seven languages: **English, Nyanja, Bemba, Lozi, Tonga, Shona and Ndebele**.

It launches a small web app with two tabs:

- **Translator**: translate between the supported languages, with automatic detection of the input language.
- **Chatbot**: ask maternal-health questions in any supported language. Answers come from [MedGemma](https://huggingface.co/google/medgemma-4b-it) with the [`dawahealth/medgemma-maternal-lora-v2`](https://huggingface.co/dawahealth/medgemma-maternal-lora-v2) adapter.

> **Not a medical device.** The chatbot can give incorrect medical information and is intended for research and development only. Have clinicians and native speakers review its outputs before any patient-facing use.

## Quick start

1. Click the **Open in Colab** badge above.
2. Choose **Runtime → Change runtime type → T4 GPU** (or better).
3. Create a Hugging Face token and add it as a Colab secret named `HF_TOKEN` (step-by-step instructions are inside the notebook).
4. Accept the terms on the gated model pages:
   - [google/medgemma-4b-it](https://huggingface.co/google/medgemma-4b-it)
   - [google/translategemma-4b-it](https://huggingface.co/google/translategemma-4b-it) (only needed if you keep the optional TranslateGemma engine; set `LOAD_TRANSLATEGEMMA = False` to skip it)
5. Run the install cell, restart the session when prompted, then run the remaining cells in order.

## How it works

| Component | Model | Role |
|---|---|---|
| Language detection | [GlotLID](https://huggingface.co/cis-lmu/glotlid) | Identifies which of the 7 languages the user wrote in |
| Translation | [NLLB-200](https://huggingface.co/facebook/nllb-200-distilled-1.3B) (default), TranslateGemma (optional) | Nyanja, Bemba, Shona and Ndebele ↔ English |
| Chatbot | MedGemma 4B + maternal LoRA | Answers the question |

### Language coverage

| Language | Translation | Notes |
|---|---|---|
| English | n/a | |
| Nyanja, Bemba, Shona | NLLB-200 | |
| Ndebele | NLLB-200 via Zulu | NLLB has no Ndebele, so Zulu is used as an approximation |
| Lozi, Tonga | Not available | Reported as unavailable rather than guessed. The chatbot can still answer directly through the adapter, with unverified quality |

### Chatbot modes

- **Auto (default):** Shona, Nyanja and Bemba questions are translated to English, answered by MedGemma, and translated back, because the adapter's own free-text answers in those languages are weak. Other languages are answered directly.
- **Direct:** MedGemma answers in the user's language for every language.
- **Always via English:** translate, answer in English, translate back, for every language that has a translator.

Optional features: show an English version of each reply for checking, switch the adapter off to compare with base MedGemma, and ground answers in your own guideline files (`.txt`, `.md`, `.pdf`) placed in `/content/guidelines`.

## Limitations

- Machine translation for these low-resource languages is imperfect. Treat it as a draft for a native speaker to check.
- The adapter's model card reports weak free-text quality in Shona, Nyanja and Bemba, repetition loops, and at least one clinically wrong English answer.
- Lozi and Tonga have no translation support here.
- The notebook was designed for a free-tier Colab T4 GPU; models are loaded in 4-bit to fit in memory.

## Licences and terms

- **Adapter:** `dawahealth/medgemma-maternal-lora-v2` is released under AGPL-3.0 and also subject to the [Health AI Developer Foundations terms](https://developers.google.com/health-ai-developer-foundations/terms). Its model card notes that offering it to others over a network (for example a shared web link) requires making the corresponding source available under AGPL-3.0.
- **NLLB-200:** weights are CC-BY-NC (non-commercial use only).
- **Gemma / TranslateGemma / MedGemma:** subject to Google's terms of use on their model pages.
- Check each model page for the current terms before deploying.

## Repository contents

```
notebooks/multilingual_maternal_health_assistant.ipynb   The Colab notebook
README.md
```

## Acknowledgements

[Dawa Health](https://dawa-health.com/) for the maternal-health adapter, Google for MedGemma and TranslateGemma, Meta for NLLB-200, and the GlotLID authors.
