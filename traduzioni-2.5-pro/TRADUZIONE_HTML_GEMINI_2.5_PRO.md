# 🌐 TRADUZIONE PDF → HTML con Gemini 2.5 Pro

## 📌 Link Notebook
```
https://colab.research.google.com/github/GoogleCloudPlatform/generative-ai/blob/main/gemini/getting-started/intro_gemini_2_5_pro.ipynb?authuser=7&hl=pt-br
```

---

## ⚠️ DIFFERENZA CON GOOGLE AI STUDIO

**Google AI Studio:**
- ✅ GRATIS
- ✅ Output HTML spettacolare
- ❌ Non sempre preciso

**Questo Notebook (Gemini 2.5 Pro via Colab):**
- ⚠️ Richiede Google Cloud Project (a pagamento)
- ✅ Più preciso e affidabile
- ✅ Output HTML possibile

---

## 🔧 SETUP INIZIALE (Da Fare UNA SOLA VOLTA)

### 1️⃣ Crea Google Cloud Project

1. Vai su: https://console.cloud.google.com
2. Crea un nuovo progetto
3. Abilita l'API Vertex AI: https://console.cloud.google.com/flows/enableapi?apiid=aiplatform.googleapis.com
4. Copia l'**ID del progetto**

### 2️⃣ Esegui le Prime Celle di Setup

Nel notebook, nelle prime celle:

1. **CELLA 1**: Installa pacchetti
   ```python
   %pip install --upgrade --quiet google-genai
   ```
   → Clicca ▶️

2. **CELLA 2**: Autenticazione
   ```python
   import sys
   if "google.colab" in sys.modules:
       from google.colab import auth
       auth.authenticate_user()
   ```
   → Clicca ▶️ e **segui le istruzioni di autenticazione**

3. **CELLA 3**: Imposta il Project ID
   ```python
   import os
   PROJECT_ID = "IL-TUO-PROJECT-ID"  # ← METTI QUI L'ID DEL TUO PROGETTO
   LOCATION = "global"
   ```
   → **SOSTITUISCI** `"IL-TUO-PROJECT-ID"` con l'ID che hai copiato
   → Clicca ▶️

4. **CELLA 4**: Import librerie
   ```python
   from IPython.display import HTML, Image, Markdown, display
   from google import genai
   from google.genai.types import (
       FunctionDeclaration,
       GenerateContentConfig,
       GoogleSearch,
       HarmBlockThreshold,
       HarmCategory,
       Part,
       SafetySetting,
       ThinkingConfig,
       Tool,
       ToolCodeExecution,
   )
   ```
   → Clicca ▶️

5. **CELLA 5**: Crea il client
   ```python
   client = genai.Client(vertexai=True, project=PROJECT_ID, location=LOCATION)
   MODEL_ID = "gemini-2.5-pro"
   ```
   → Clicca ▶️

---

## 📤 CELLA CARICAMENTO PDF (+ Code)

Clicca **"+ Codice"** e incolla:

```python
from google.colab import files
import os

print("📤 Seleziona i file PDF da caricare...")
uploaded = files.upload()

pdf_files = [f for f in uploaded.keys() if f.endswith('.pdf')]
print(f"\n✅ Caricati {len(pdf_files)} file PDF:")
for f in pdf_files:
    print(f"  - {f}")
```

Clicca ▶️ e seleziona i 3 PDF.

---

## 🌍 CELLA TRADUZIONE → HTML (+ Code)

Clicca **"+ Codice"** e incolla:

```python
traduzioni_html = {}

for pdf_file in pdf_files:
    print(f"\n📄 Traduzione: {pdf_file}")
    
    # Leggi il PDF
    with open(pdf_file, "rb") as f:
        file_bytes = f.read()
    
    # Traduci in HTML
    response = client.models.generate_content(
        model=MODEL_ID,
        contents=[
            """Translate this document to English. 
            Output the translation in clean HTML format.
            Preserve the document structure using appropriate HTML tags (h1, h2, p, ul, li, etc.).
            Make it visually appealing and well-formatted.""",
            Part.from_bytes(data=file_bytes, mime_type="application/pdf"),
        ],
        config=GenerateContentConfig(
            temperature=0.2,
        ),
    )
    
    # Salva come HTML
    traduzioni_html[pdf_file] = response.text
    print(f"✅ Completato: {pdf_file}")

print(f"\n🎉 Tradotti {len(traduzioni_html)} documenti in HTML!")
```

Clicca ▶️ e aspetta.

---

## 💾 CELLA DOWNLOAD HTML (+ Code)

Clicca **"+ Codice"** e incolla:

```python
for pdf_file, html_content in traduzioni_html.items():
    output_file = pdf_file.replace('.pdf', '_tradotto.html')
    
    # Salva come HTML
    with open(output_file, 'w', encoding='utf-8') as f:
        # Aggiungi header HTML base
        f.write('<!DOCTYPE html>\n<html>\n<head>\n')
        f.write('<meta charset="UTF-8">\n')
        f.write('<title>Traduzione</title>\n')
        f.write('<style>body { font-family: Arial, sans-serif; margin: 40px; }</style>\n')
        f.write('</head>\n<body>\n')
        f.write(html_content)
        f.write('\n</body>\n</html>')
    
    print(f"💾 Salvato: {output_file}")

# Download
print("\n📥 Download dei file HTML...")
for pdf_file in traduzioni_html.keys():
    output_file = pdf_file.replace('.pdf', '_tradotto.html')
    files.download(output_file)

print("\n✨ Processo completato! File HTML pronti.")
```

Clicca ▶️ e i file HTML si scaricheranno.

---

## ✅ RISULTATO

Ora hai i PDF tradotti in **HTML** (non TXT)!

Puoi aprirli con un browser (Chrome, Firefox) e vedere la formattazione.

---

## ⚠️ NOTA IMPORTANTE

Questo metodo **NON È GRATIS** come Google AI Studio.

Richiede un Google Cloud Project con fatturazione abilitata.

Se vuoi gratis, usa Google AI Studio (anche se meno preciso).