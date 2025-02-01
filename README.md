## Join the Community
[Join Curious PM Community](https://curious.pm) to connect, share, and learn with others!

# Speech-to-Text Transcription App

This project is a **Streamlit web app** that allows users to upload an audio file (e.g., `.mp3`, `.wav`, `.m4a`) and convert it into text using a **Whisper API**. The app is interactive and displays the transcribed text, making it easy to process audio recordings.

---

## What the Code Does
This application enables users to:

- **Audio Upload:** Upload an audio file through the Streamlit interface.
- **Transcription:** Send the uploaded file to a Speech-to-Text API and receive transcribed text.
- **User Review:** Display the transcribed output for user review, with error-handling features.

---

## How to Implement It (Step by Step)

### 1. Set Up Your Environment
Start by creating a new project directory and setting up a Python virtual environment.
```bash
mkdir speech_to_text_app
cd speech_to_text_app
python3 -m venv venv
source venv/bin/activate  # On Windows: .\venv\Scripts\activate
```

### 2. Install Required Dependencies
Create a `requirements.txt` file with the following content:
```
streamlit
requests
```
Then install the dependencies using:
```bash
pip install -r requirements.txt
```

### 3. Create the Main App File
Inside your project directory, create a file called `app.py`. Add the following code to set up the Streamlit interface:

```python
import streamlit as st
import requests

# Streamlit UI setup
st.title("Speech-to-Text Transcription App")

# File uploader
uploaded_file = st.file_uploader("Upload an audio file", type=["mp3", "wav", "m4a"])

if uploaded_file is not None:
    st.audio(uploaded_file, format="audio/mp3")
    if st.button("Transcribe Audio"):
        with st.spinner("Transcribing audio..."):
            response = transcribe_audio(uploaded_file)
            if response:
                st.text_area("Transcribed Text", response)
            else:
                st.error("Failed to transcribe the audio.")

# Function to send audio to API
def transcribe_audio(audio_file):
    api_url = "YOUR_API_URL"
    api_key = "YOUR_API_KEY"
    files = {"file": audio_file.read()}
    headers = {"Authorization": f"Bearer {api_key}"}
    try:
        response = requests.post(api_url, headers=headers, files=files)
        response.raise_for_status()
        return response.json().get("transcription", "No transcription available.")
    except Exception as e:
        st.error(f"Error: {e}")
        return None
```

### 4. Configure API Credentials
Create a `secrets.toml` file in your project directory to store sensitive API details.
```toml
[default]
api_url = "YOUR_API_URL"
api_key = "YOUR_API_KEY"
```
Alternatively, you can set them as environment variables to enhance security.

### 5. Run the Streamlit App
Run the app locally using Streamlit:
```bash
streamlit run app.py
```
Navigate to the link provided in the terminal to access the app interface.

---

## What Happens in Each Step

1. **User Uploads Audio File:** The app’s file uploader allows users to select an audio file (supported formats: `.mp3`, `.wav`, `.m4a`). The file is played using Streamlit’s audio player.

2. **API Request:** Once the file is uploaded, the app sends it to the Speech-to-Text API through an HTTP POST request, including necessary headers for authentication.

3. **API Processing:** The API processes the audio and returns the transcribed text as a JSON response.

4. **Displaying Results:** The transcribed text is displayed in a text area on the interface. If an error occurs, the app shows an appropriate error message.

---

## Folder Structure
```
speech_to_text_app/
├── app.py                  # Main Streamlit app
├── requirements.txt        # Python dependencies for the project
├── secrets.toml            # Contains API URL and API key (do not share publicly)
└── .gitignore              # Ignore unnecessary files in the repo
```

### Explanation of Files
- **`app.py`**: The main Streamlit app, handling user input, API interaction, and displaying the transcription.
- **`requirements.txt`**: Lists required Python libraries.
- **`secrets.toml`**: Contains sensitive data (API URL and API key) for safer configuration management.
- **`.gitignore`**: Ensures sensitive files and unnecessary dependencies are not tracked in the version control.

---

## Screenshots
![Screenshot 2025-01-30 224207](https://github.com/user-attachments/assets/f64acad4-acbf-4041-a0be-b0c856c623a1)

---

## Security Considerations
- **Do not share your API keys.**
- Use environment variables or secrets files for production.
- Regularly update and rotate API keys to enhance security.

