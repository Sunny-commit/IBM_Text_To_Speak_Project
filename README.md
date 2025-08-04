

# IBM Text To Speak Project

This project demonstrates how to use the **IBM Watson Text-to-Speech API** to convert text into professional, human-like speech. It uses **Python** and **Speech Synthesis Markup Language (SSML)** to precisely control the voice's tone, pacing, and emphasis, making it an excellent tool for generating high-quality audio for advertisements and other content.

## 🚀 Project Structure

The project has a simple and clean structure to get you up and running quickly.

```
IBM_Text_To_Speak_Project/
├── voice folder
├── README.md                 ← This file
├── audio_file.ipynb                 ← The core Python script
└── audio_file.pdf
```

## 🧰 Features

  * **Expressive Neural Voice**: Utilizes IBM's advanced `en-US_MichaelExpressive` voice, which is designed for a dynamic, conversational style.
  * **SSML Integration**: The ad copy is annotated with SSML tags to control pauses, pitch, and emphasis, ensuring a natural and impactful delivery.
  * **Python-based**: A single, self-contained Python script makes it easy to generate new audio files.
  * **API-Driven**: This project provides a direct, code-based solution for interacting with the IBM Cloud Text-to-Speech service.

## ⚙️ Installation

1.  **Clone the repository**:

    ```bash
    git clone https://github.com/Sunny-commit/IBM_Text_To_Speak_Project.git
    cd IBM_Text_To_Speak_Project
    ```

2.  **Install Python dependencies**:

    ```bash
    pip install -r requirements.txt
    ```

3.  **Get your IBM Cloud credentials**:

      * Sign up for a free [IBM Cloud account](https://cloud.ibm.com/registration).
      * Create an instance of the [Text-to-Speech service](https://cloud.ibm.com/catalog/services/text-to-speech).
      * Copy your **API Key** and the service **URL**.

4.  **Update the script**:
    Open `main.py` and replace the placeholder values with your actual credentials:

    ```python
    api_key_placeholder = "{API_KEY}"
    url_placeholder = "{URL}"
    ```

    Change these lines to:

    ```python
    api_key_placeholder = "YOUR_ACTUAL_API_KEY"
    url_placeholder = "YOUR_ACTUAL_URL"
    ```

## 🎬 Usage

Run the script directly from your terminal. It will generate a `.wav` audio file in your project directory.

```bash
python main.py
```

This will output `voice.wav` with the synthesized speech.

-----

### Understanding the SSML Tags

The script's professional voice delivery is achieved through **Speech Synthesis Markup Language (SSML)**. Here's a breakdown of the key tags used and their functions:

  * **`<express-as style="neutral">`**: Sets the overall emotional tone of the voice. The `"neutral"` style is ideal for a professional, objective, and confident delivery.
  * **`<prosody rate="..." pitch="...">`**: Adjusts the speaking speed (`rate`) and tone (`pitch`) for specific sections. For expressive voices, these require percentage values (e.g., `-15%` for slower, `+15%` for faster/higher).
  * **`<break time="...">`**: Inserts a pause of a specified duration (e.g., `1s` for one second). This is vital for natural pacing and dramatic effect.
  * **`<emphasis level="strong">`**: Adds extra stress and volume to a word or phrase, mimicking a human voice actor's emphasis.

-----

### The Python Code if you want to try it by your self.

The core script, `main.py`, is provided below. You can update the `ssml_text` variable to generate audio for your own ad copy.


```python
import requests
import json
import base64
from pathlib import Path

def convert_ad_to_speech(api_key, url, output_filename="ad_audio.wav"):
    """
    Converts ad copy to a WAV audio file using IBM Watson Text-to-Speech API
    with SSML for a professional, human-like delivery.

    Args:
        api_key (str): Your IBM Watson API key.
        url (str): The IBM Watson service URL.
        output_filename (str, optional): The name of the output audio file.
                                         Defaults to "ad_audio.wav".
    """
    # The SSML text is hardcoded to ensure consistent ad delivery.
    # It includes tags for pauses (<break>), emphasis (<emphasis>), and
    # expressive style (<express-as>) to make the voice sound natural.
    ssml_text = """
<speak>
    <express-as style="neutral">
        <prosody rate="-15%">
            <emphasis level="strong">🚨 You’re One PDF Away From Changing Everything 🚨</emphasis>
        </prosody>
        <break time="1s"/>

        <prosody rate="0%">
            Tired of searching endlessly online, only to find outdated, half-baked content?
        </prosody>
        <break time="700ms"/>

        <p>
            <prosody pitch="-10%">This PDF is not just another download.</prosody><break time="400ms"/>
            It’s a <emphasis level="strong">shortcut</emphasis> to what you <emphasis level="strong">actually need</emphasis>.
        </p>
        <break time="1s"/>

        <prosody rate="0%">
            <prosody pitch="-10%"><emphasis level="strong">🔒 Why This PDF?</emphasis></prosody>
            <break time="700ms"/>
            <p>
                <emphasis level="strong">✅ Proven strategies</emphasis> you won’t find for free<break time="300ms"/>
                <emphasis level="strong">✅ Actionable steps</emphasis> – no fluff, just results<break time="300ms"/>
                <emphasis level="strong">✅ Real-world value</emphasis> in every single page<break time="300ms"/>
                <emphasis level="strong">✅ Exclusive insights</emphasis> from [Your Niche/Field] experts
            </p>
        </prosody>
        <break time="1s"/>

        <p>
            This is the <emphasis level="strong">exact resource</emphasis> that <prosody rate="-15%">[thousands/yourself]</prosody> used to <prosody pitch="+5%">[solve a problem/gain success]</prosody>—and now, it’s <emphasis level="strong">your turn</emphasis>.
        </p>
        <break time="1s"/>

        <prosody rate="+15%">
            <emphasis level="strong">⏳ Limited-time launch offer:</emphasis> Get it now before the <emphasis level="strong">price doubles</emphasis>.
        </prosody>
        <break time="700ms"/>

        <prosody pitch="+5%" rate="-15%">
            🎁 <emphasis level="strong">Bonus inside</emphasis> for first <emphasis level="strong">100 downloads</emphasis>.
        </prosody>
        <break time="1s"/>

        <prosody pitch="-10%">
            💬 Real buyers say:<break time="500ms"/>
            <p>"I wish I had this sooner."</p><break time="500ms"/>
            <p>"It’s worth 10x the price."</p>
        </prosody>
        <break time="1s"/>

        <prosody rate="0%">
            🛒 Ready to <emphasis level="strong">stop guessing</emphasis> and start <emphasis level="strong">winning</emphasis>?
        </prosody>
        <break time="700ms"/>

        <prosody rate="-15%"><emphasis level="strong">Click Buy Now.</emphasis></prosody> You won’t regret it.
        <break time="1s"/>

        <prosody rate="+15%">
            📥 <emphasis level="strong">Instant access</emphasis>. No waiting. No BS.
        </prosody>
    </express-as>
</speak>
"""
    # Using an Expressive Neural Voice for speaking styles and a professional tone
    voice = "en-US_MichaelExpressive"
    full_url = f"{url}/v1/synthesize?voice={voice}"

    # Prepare authentication (Basic Auth with API Key)
    auth_string = f"apikey:{api_key}"
    encoded_auth = base64.b64encode(auth_string.encode("utf-8")).decode("utf-8")
    
    # Headers for the API call
    headers = {
        "Content-Type": "application/json",
        "Accept": "audio/wav",
        "Authorization": f"Basic {encoded_auth}"
    }
    
    # Create the JSON payload
    payload = {
        "text": ssml_text
    }

    print(f"Synthesizing audio for ad text...")
    try:
        response = requests.post(full_url, headers=headers, data=json.dumps(payload), stream=True)
        response.raise_for_status()  # Raise an HTTPError for bad responses (4xx or 5xx)

        # Save the audio file
        with open(output_filename, 'wb') as audio_file:
            for chunk in response.iter_content(chunk_size=1024):
                audio_file.write(chunk)

        print(f"Audio saved to '{output_filename}'")
    except requests.exceptions.RequestException as e:
        print(f"Error during API call: {e}")
        if response is not None:
            print(f"Status Code: {response.status_code}")
            print(f"Response Body: {response.text}")

# --- How to run this code in a Python environment like Google Colab ---
if __name__ == '__main__':
    # You must provide your own API key and URL here.
    # Replace "YOUR_API_KEY" and "YOUR_URL" with your actual credentials.
    # Note: The script is already configured with your API key and URL for this example.
    api_key_placeholder = "{Enter your API KEY}"
    url_placeholder = "{Enter your service URL}"
    
    convert_ad_to_speech(api_key=api_key_placeholder, url=url_placeholder, output_filename="instagram_ad.wav")
```

-----

### Example Output

To demonstrate the high-quality audio generated by this script, you can host the output `.wav` file and embed it in your `README`.

```html
<audio controls>
  <source src="https://example.com/path/to/your/instagram_ad.wav" type="audio/wav">
  Your browser does not support the audio element.
</audio>
```
