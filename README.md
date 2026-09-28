# SignBridge

**Real-time, two-way sign language ↔ speech translation that runs entirely in the browser.**

- **Sign → Speech:** webcam hand tracking (MediaPipe) → sign classifier → text → spoken aloud (Web Speech API). Seven gestures work instantly with no training, and you can teach your own.
- **Speech → Text:** microphone → live captions the signer can read.

No server, no install, no data leaves your machine.

## Demo

Demo video: [link coming soon]

## Tech stack

- **JavaScript** (vanilla ES modules, single `index.html`, no build step)
- **MediaPipe Gesture Recognizer** – 21 3D hand keypoints per hand plus pre-trained gesture classification, GPU-accelerated in the browser
- **k-nearest-neighbours (k-NN)** – lightweight classifier trained live on signs you record
- **Web Speech API** – `SpeechSynthesis` for text-to-speech, `SpeechRecognition` for live captions

## Run locally

Camera and microphone need `localhost` or HTTPS, so opening `index.html` directly from disk won't work. Use either option below, then open the page in **Chrome or Edge**.

**Option 1 – VS Code + Live Server**

1. Install the [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) extension.
2. Open this folder in VS Code, right-click `index.html` → **Open with Live Server**.

**Option 2 – Python**

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

## Using it

1. Allow camera access. Your hands appear with a skeleton overlay.
2. Try a built-in gesture right away (hold it for a moment):

   | Gesture | Word |
   |---|---|
   | ✋ Open palm | hello |
   | ✊ Closed fist | yes |
   | 👍 Thumb up | good |
   | 👎 Thumb down | bad |
   | ✌️ Victory | peace |
   | ☝️ Pointing up | wait |
   | 🤟 I love you | I love you |

   Built-in gestures take priority over taught signs. Untick **built-in gestures** to use only your own.
3. Under **Teach a sign**, type a word, make the sign and hold **Hold to record** for ~3 s. Record 3+ words plus one called `_rest` (hands relaxed).
4. Sign. Recognised words build a sentence and are spoken aloud.
5. Click **Start listening** to get live captions of what the hearing person says.

**Saving and loading signs:** click **Save dataset** to download your recorded signs as `signs-dataset.json`. Next time, click **Load dataset** and pick that file to skip re-teaching.

## How it works

1. **Hand tracking** – MediaPipe Gesture Recognizer finds 21 3D keypoints per hand, for up to two hands, each frame, and labels 7 common gestures with a pre-trained model.
2. **Features** – keypoints are made relative to the wrist and scaled, giving a 126-number vector that ignores where you stand.
3. **Classification** – if a built-in gesture is detected with ≥60% confidence it is used; otherwise k-nearest-neighbours compares the vector to signs you've recorded.
4. **Smoothing** – a sign must be held ~12 frames with 80% agreement before it becomes a word; a `_rest` class marks pauses.
5. **Output** – accepted words build a sentence and are spoken with text-to-speech.

## Status / roadmap

- [x] Baseline: static signs taught by the user, speech captions
- [x] Pre-trained gestures that work with no training (MediaPipe Gesture Recognizer)
- [ ] Motion signs (sequence model, e.g. LSTM/Transformer over landmark sequences)
- [ ] Pre-trained vocabulary from a public dataset (WLASL for ASL, LSE datasets for Spanish Sign Language)
- [ ] Grammar: sign gloss → natural sentence with an LLM
- [ ] Speech → signing avatar
- [ ] Mobile app

## License

[MIT](LICENSE)
