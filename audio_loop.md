# AudioLoop

**AudioLoop** is a Python module designed for real-time audio, video, and text streaming, enabling seamless bi-directional communication with Google's Gemini AI model. Leveraging asynchronous programming with `asyncio`, `AudioLoop` facilitates real-time audio playback, video capture, and textual interactions, making it an ideal choice for applications requiring interactive AI-driven multimedia capabilities.  
The code is adapted from the Gemini 2.0 cookbook example: live_api_starter.py. Please check the References below.  

## Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
  - [Importing the AudioLoop Class](#importing-the-audioloop-class)
  - [Initializing AudioLoop](#initializing-audioloop)
  - [Running the AudioLoop](#running-the-audioloop)
- [CLI Application](#cli-application)
- [Logging](#logging)
- [Configuration](#configuration)
- [Dependencies](#dependencies)
- [License](#license)
- [References](#references)

## Features

- **Real-Time Audio Streaming**: Capture audio from the microphone and play back audio responses from the AI model.
- **Video Capture**: Stream video frames from the camera in real-time.
- **Screen Capture**: Capture and stream screenshots of the primary display.
- **Textual Interaction**: Send and receive text messages to and from the AI model.
- **Asynchronous Operations**: Utilizes `asyncio` for managing concurrent tasks efficiently.
- **Logging**: Comprehensive logging to monitor and debug the application's behavior.
- **Extensible**: Designed to be integrated into other programs managing GUI components.

## Prerequisites

- **Python**: Version 3.11 or higher is required.
- **Google GenAI Account**: Access to Google's Gemini AI model with appropriate API credentials.

## Installation

1. **Clone the Repository**

   ```bash
   git clone https://github.com/yourusername/audioloop.git
   cd audioloop
   ```

2. **Create a Virtual Environment (Optional but Recommended)**

   ```bash
   python3 -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install Dependencies**

   Ensure you have `pip` updated:

   ```bash
   pip install --upgrade pip
   ```

   Install the required packages:

   ```bash
   pip install asyncio pyaudio opencv-python mss Pillow python-dotenv google-genai
   ```

   > **Note**: `pyaudio` may require additional system dependencies. Refer to the [PyAudio Installation Guide](https://people.csail.mit.edu/hubert/pyaudio/#downloads) for platform-specific instructions.

4. **Set Up Environment Variables**

   Create a `.env` file in the project root directory and add your Google GenAI API credentials:

   ```env
   GEMINI_API_KEY=your_api_key_here
   ```

## Usage

### Importing the AudioLoop Class

To use the `AudioLoop` class in your project, import it from the `audio_loop` module:

```python
import asyncio
from audio_loop import AudioLoop
from google import genai

# Initialize your GenAI client
client = genai.Client(http_options={"api_version": "v1alpha"})
```

### Initializing AudioLoop

Create an instance of `AudioLoop` by providing an `asyncio.Queue` for user inputs and an optional callback for displaying text responses:

```python
user_input_queue = asyncio.Queue()

def display_text(text):
    print(f"AI: {text}")

audio_loop = AudioLoop(user_input_queue=user_input_queue, display_text_callback=display_text)
```

### Running the AudioLoop

Run the `AudioLoop` within an asynchronous event loop, specifying the AI model, configuration, input mode, and GenAI client:

```python
async def main():
    model = "models/gemini-2.0-flash-exp"
    config = {
        "generation_config": {
            "response_modalities": ["AUDIO"],
            "speech_config": "Kore"  # Example voice
        }
    }
    mode = "camera"  # Options: "text", "camera", "screen"
    
    await audio_loop.run(model=model, config=config, mode=mode, client=client)

if __name__ == "__main__":
    asyncio.run(main())
```

## CLI Application

The `audio_loop.py` script includes a command-line interface (CLI) that allows you to run the `AudioLoop` directly. To use the CLI:

1. **Run the Script**

   ```bash
   python audio_loop.py --mode camera
   ```

   **Arguments:**

   - `--mode`: Specifies the source of video frames to stream. Options are:
     - `text` (default): Text-only interaction.
     - `camera`: Stream video from the default camera.
     - `screen`: Stream screenshots of the primary display.

2. **Interact via Console**

   - **Send Messages**: Type your messages after the `message >` prompt and press Enter.
   - **Exit**: Type `quit` or `q` to terminate the application gracefully.

## Logging

Logging is configured to provide detailed information about the application's operations, aiding in debugging and monitoring.

- **Log Configuration**: Logs are set up using the `setup_logging()` function.
- **Log Files**: Log files are stored in the `logs` directory with timestamps in their filenames.
- **Log Levels**: The default log level is set to `DEBUG` for comprehensive logging. Adjust as needed in the `setup_logging` function.
- **Console Logging**: By default, logs are written to files only. To enable console logging, uncomment the `StreamHandler` line in the `setup_logging()` function.

## Configuration

Customize the AI model and response modalities by modifying the configuration dictionaries:

- **Text Response Only**

  ```python
  CONFIG_TEXT = {
      "generation_config": {
          "response_modalities": ["TEXT"]
      }
  }
  ```

- **Audio Response**

  ```python
  voices = ["Puck", "Charon", "Kore", "Fenrir", "Aoede"]
  CONFIG_AUDIO = {
      "generation_config": {
          "response_modalities": ["AUDIO"],
          "speech_config": voices[2]  # Example: "Kore"
      }
  }
  ```

Select the desired configuration when initializing the `AudioLoop`.

## Dependencies

The `AudioLoop` module relies on the following Python packages:

- **Standard Libraries**:
  - `asyncio`
  - `logging`
  - `os`
  - `datetime`
  - `base64`
  - `io`
  - `traceback`
  - `argparse`

- **Third-Party Libraries**:
  - [`pyaudio`](https://people.csail.mit.edu/hubert/pyaudio/) - Audio input/output.
  - [`opencv-python`](https://pypi.org/project/opencv-python/) - Video capture and processing.
  - [`mss`](https://pypi.org/project/mss/) - Screen capturing.
  - [`Pillow`](https://pypi.org/project/Pillow/) - Image processing.
  - [`python-dotenv`](https://pypi.org/project/python-dotenv/) - Environment variable management.
  - [`google-genai`](https://pypi.org/project/google-genai/) - Interaction with Google's Gemini AI model.

Ensure all dependencies are installed via `pip` as outlined in the [Installation](#installation) section.

## License

[MIT License](LICENSE)

## References

https://github.com/google-gemini/cookbook/blob/main/gemini-2/README.md  
https://github.com/google-gemini/cookbook/blob/main/gemini-2/live_api_starter.py  

---
