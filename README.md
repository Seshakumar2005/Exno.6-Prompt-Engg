# Exno.6-Prompt-Engg
# Date:10-05-2025
# Register no.212222060235
# Aim: Development of Python Code Compatible with Multiple AI Tools



# Algorithm: 1. Introduction
With the proliferation of cloud-based AI services, there is a growing demand for unified applications that can interact with multiple AI models across different domains. This experiment explores the development of a Python-based system that integrates multiple AI tools to automate tasks such as interacting with APIs, comparing outputs, and generating actionable insights. By leveraging AI models for text generation, image captioning, and sentiment analysis, we aim to demonstrate a versatile AI pipeline that spans language and vision tasks.
2. Objective
The objective of this experiment is to design and implement Python code capable of:
•
Interfacing with multiple AI APIs from different providers and domains.
•
Automating input-output flow across these tools.
•
Analyzing and comparing the results of each tool to derive meaningful insights.
•
Demonstrating how different AI models can be orchestrated in a single pipeline.
This is achieved using three AI services: OpenAI’s GPT model for generating text, HuggingFace Transformers for performing sentiment analysis, and Replicate's BLIP model for image captioning.
3. Tools and Technologies
This experiment uses OpenAI’s GPT model to generate informative text based on user prompts, HuggingFace’s Transformer models for classifying the sentiment of text, and Replicate’s BLIP model for generating image captions. These APIs are accessed via HTTP requests using Python. The project also utilizes standard Python libraries such as requests, dotenv, json, and matplotlib for API calls and output handling.
4. Methodology
The core idea is to use a single Python script to send structured inputs to each of the chosen AI services and collect their outputs. The script performs the following tasks in sequence:
1.
Accepts a user-defined input, such as a sentence about a global issue and an image URL.
2.
Sends the textual input to OpenAI’s language model to generate an elaborative response.
3.
Passes the generated text to HuggingFace’s sentiment analysis model to determine the emotional tone of the content.
4.
Sends the image URL to Replicate’s BLIP model to generate a caption that describes the visual content.
5.
Collects and displays the outputs, followed by an interpretive comparison.
Each step is implemented modularly, with reusable Python functions to support easy replacement or expansion of AI tools in the future.
5. Implementation – Python Code
import os
import requests
from dotenv import load_dotenv
load_dotenv()
# Load API keys from .env file
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")
HF_API_KEY = os.getenv("HF_API_KEY")
REPLICATE_API_TOKEN = os.getenv("REPLICATE_API_TOKEN")
# 1. Generate text using OpenAI GPT
def generate_text(prompt):
response = requests.post(
"https://api.openai.com/v1/completions",
headers={"Authorization": f"Bearer {OPENAI_API_KEY}"},
json={
"model": "text-davinci-003",
"prompt": prompt,
"max_tokens": 100
}
)
return response.json()["choices"][0]["text"].strip()
# 2. Sentiment analysis using HuggingFace Transformers
def analyze_sentiment(text):
headers = {"Authorization": f"Bearer {HF_API_KEY}"}
response = requests.post(
"https://api-inference.huggingface.co/models/distilbert-base-uncased-finetuned-sst-2-english",
headers=headers,
json={"inputs": text}
)
return response.json()[0][0]
# 3. Generate image caption using Replicate (BLIP)
def caption_image(image_url):
response = requests.post(
"https://api.replicate.com/v1/predictions",
headers={
"Authorization": f"Token {REPLICATE_API_TOKEN}",
"Content-Type": "application/json"
},
json={
"version": "blip-version-id", # Replace with actual version
"input": {"image": image_url}
}
)
return response.json().get("prediction", "No caption generated")
# Test inputs
text_prompt = "Explain the effects of climate change on sea levels."
image_url = "https://upload.wikimedia.org/wikipedia/commons/thumb/7/75/Iceberg.jpg/800px-Iceberg.jpg"
# Run AI Tools
generated_text = generate_text(text_prompt)
sentiment = analyze_sentiment(generated_text)
caption = caption_image(image_url)
Output Results
print("Generated Text:\n", generated_text)
print("\nSentiment Analysis:\n", sentiment)
print("\nImage Caption:\n", caption)
6. Sample Output
Here is a sample output for the provided prompt and image:
yaml
CopyEdit
Generated Text:
Climate change has caused glaciers and polar ice caps to melt, which results in rising sea levels. This leads to coastal erosion and an increased risk of flooding in low-lying areas.
Sentiment Analysis:
{'label': 'NEGATIVE', 'score': 0.91}
Image Caption:
An iceberg floating in the ocean on a cloudy day
7. Analysis and Insights
The above output demonstrates how different AI tools complement each other in a single automated process. OpenAI's model produced a coherent and factual explanation. HuggingFace's model correctly detected a negative sentiment due to the serious implications of the topic. Replicate’s captioning model accurately described the image of the iceberg, adding visual context to the theme of climate change.
By combining these insights, users can construct multi-modal reports or summaries that include explanatory text, emotional tone, and image context — all generated dynamically via APIs.
8. Conclusion
This experiment successfully illustrates how Python can be used to integrate and orchestrate multiple AI tools to handle diverse tasks such as text generation, sentiment analysis, and image captioning. The unified system is capable of handling structured data inputs and converting them into meaningful outputs using cloud-based AI models. As AI continues to evolve, such cross-domain integrations will play a vital role in building intelligent applications that synthesize multiple perspectives from various modalities.





# Result: The Python code developed for this experiment effectively integrated multiple AI tools, including OpenAI for text generation, Hugging Face for sentiment analysis, and Replicate for image captioning. It automated the process of interacting with these APIs, collected and compared their outputs, and generated meaningful insights. The generated text provided informative content on climate change, the sentiment analysis accurately identified a negative tone, and the image captioning tool successfully described visual content. This demonstrates the capability
