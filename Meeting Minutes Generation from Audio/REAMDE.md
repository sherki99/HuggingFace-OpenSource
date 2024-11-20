# Meeting Minutes Generation from Audio

This project uses OpenAI's Whisper model and other powerful tools to convert audio from Denver City Council meetings into structured meeting minutes. The generated minutes include a summary, key discussion points, takeaways, and action items with owners.

## Objective

The goal of this project is to convert audio recordings of meetings into comprehensive meeting minutes automatically, which include:
- **Summary** of the meeting.
- **Key discussion points**.
- **Takeaways**.
- **Action items** with owners.

The project uses Hugging Face models and OpenAI API for audio transcription and language generation.

## Dataset

The dataset contains meeting audio recordings from Denver City Council meetings. The data is hosted on Hugging Face and can be accessed [here](https://huggingface.co/datasets/huuuyeah/meetingbank).

## Technologies Used

- **Whisper** by OpenAI: For audio-to-text transcription.
- **LLaMA (Meta's Language Model)**: For generating structured meeting minutes.
- **Hugging Face Transformers**: For model loading, tokenization, and pipeline integration.
- **OpenAI API**: For transcription via Whisper.
- **Google Colab**: For running the code on GPUs in the cloud.

## Setup and Installation

### 1. Install Dependencies

Run the following command to install the necessary libraries:
```bash
!pip install -q requests torch bitsandbytes transformers sentencepiece accelerate openai
```

### 2. Import Libraries

The required libraries are imported in the script:

```python
import os
import requests
from IPython.display import Markdown, display
from openai import OpenAI
from google.colab import drive
from huggingface_hub import login
from transformers import AutoTokenizer, AutoModelForCausalLM, TextStreamer, BitsAndBytesConfig
import torch
```

### 3. Mount Google Drive

To use your audio files stored in Google Drive:
```python
drive.mount("/content/drive")
```

### 4. Define Paths and Models

Set the paths for your audio files and specify the models to be used:

```python
audio_filename = "/content/drive/MyDrive/llms/denver_extract.mp3"
AUDIO_MODEL = "whisper-1"
LLAMA = "meta-llama/Meta-Llama-3.1-8B-Instruct"
PHI3 = "microsoft/Phi-3-mini-4k-instruct"
```

### 5. Authentication

Authenticate with Hugging Face and OpenAI:

```python
hf_token = userdata.get('HF_TOKEN')
login(hf_token, add_to_git_credential=True)

openai_api_key = userdata.get('OPENAI_API_KEY')
openai = OpenAI(api_key=openai_api_key)
```

### 6. Transcribe Audio to Text

Use the Whisper model to transcribe the audio into text:

```python
audio_file = open(audio_filename, "rb")
transcription = openai.audio.transcriptions.create(model=AUDIO_MODEL, file=audio_file, response_format="text")
print(transcription)
```

### 7. Generate Meeting Minutes

Once you have the transcription, generate structured meeting minutes using LLaMA (or any other suitable model). The prompt for LLaMA is configured like this:

```python
system_message = "You are an assistant that produces minutes of meetings from transcripts, with summary, key discussion points, takeaways and action items with owners, in markdown."
user_prompt = f"Below is an extract transcript of a Denver council meeting. Please write minutes in markdown, including a summary with attendees, location and date; discussion points; takeaways; and action items with owners.\n{transcription}"

messages = [
    {"role": "system", "content": system_message},
    {"role": "user", "content": user_prompt}
]
```

Finally, generate the output using the model:

```python
tokenizer = AutoTokenizer.from_pretrained(PHI3)
tokenizer.pad_token = tokenizer.eos_token

inputs = tokenizer.apply_chat_template(messages, return_tensors="pt").to(device)

streamer = TextStreamer(tokenizer)
model = AutoModelForCausalLM.from_pretrained(PHI3, device_map="auto")
outputs = model.generate(inputs, max_new_tokens=2000, streamer=streamer)

response = tokenizer.decode(outputs[0])

display(Markdown(response))
```

### 8. Alternative: Using Hugging Face Pipeline

Alternatively, you can use Hugging Face's `pipeline` for Automatic Speech Recognition (ASR):

```python
from transformers import AutoModelForSpeechSeq2Seq, AutoProcessor, pipeline

speech_model = AutoModelForSpeechSeq2Seq.from_pretrained(
    AUDIO_MODEL,
    torch_dtype=torch.float16,
    low_cpu_mem_usage=True,
    use_safetensors=True
)

device = 'cuda' if torch.cuda.is_available() else 'cpu'
speech_model.to(device)

processor = AutoProcessor.from_pretrained(AUDIO_MODEL)

pipe = pipeline(
    task="automatic-speech-recognition",
    model=speech_model,
    tokenizer=processor.tokenizer,
    feature_extractor=processor.feature_extractor,
    torch_dtype=torch.float16,
    device=0 if device == 'cuda' else -1,
)

result = pipe(audio_filename)
transcription = result["text"]
print(transcription)
```

### 9. Run the Script

Once all the code is set up, you can run the script to generate meeting minutes from the audio.

## Example Output

The output will be displayed in markdown format, including:

- **Summary**: Overview of the meeting, location, date, and attendees.
- **Discussion Points**: Key points discussed during the meeting.
- **Takeaways**: Important decisions or conclusions.
- **Action Items**: Specific tasks with owners.

Example:

```markdown
### Denver City Council Meeting Minutes

**Date**: 2024-11-20  
**Location**: City Hall  
**Attendees**: John Doe, Jane Smith, Bob Johnson

---

### Discussion Points

- **Budget Review**: The council discussed the proposed budget for the next fiscal year.
- **New City Ordinances**: The council considered a new ordinance for zoning laws.
- **Public Safety**: A proposal for increasing police patrols was introduced.

---

### Takeaways

- The budget proposal was approved unanimously.
- Zoning laws will be revisited next month.
- A task force for public safety will be created.

---

### Action Items

- **John Doe**: Draft a revised budget proposal by next meeting.
- **Jane Smith**: Research zoning law changes and report by next session.
- **Bob Johnson**: Lead the public safety task force and organize the first meeting.
```

## Conclusion

This project leverages state-of-the-art models from OpenAI, Hugging Face, and Google Colab to automate the process of generating meeting minutes from audio recordings. It uses speech recognition for transcription and advanced language models to generate structured summaries, discussion points, and action items.

---

### Notes

- The Hugging Face models used here can be substituted with other models depending on your preferences.
- The transcription process can be optimized for different audio qualities by using a different model, e.g., using larger Whisper models.
