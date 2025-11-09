# whisper-macos-metal-guide  
Offline Speech-to-Text on Intel Macs (Metal GPU)

### 🧠 Overview  
This repository provides a verified, real-world build guide for **whisper.cpp** running on Intel-based macOS systems with Metal-enabled AMD GPUs (tested with RX 580 8 GB on macOS Sequoia 15.7.2 via OCLP).  
The goal: keep legacy Intel Macs fully usable for offline AI workloads.

### ⚙️ Key Features  
- ✅ Fully offline – no OpenAI API required  
- ⚡ Verified Metal 2 acceleration on AMD RX 580  
- 🧩 macOS Sequoia 15.7.2 (OCLP patched) tested  
- 🧠 Step-by-step Homebrew + CMake build  
- 💾 Model download, quantization (q5_0) and transcription examples  
- 🪶 Optional PDF export via Pandoc + Tectonic  

### 📄 Documentation  
Full instructions:  
👉 [`Whisper_on_Intel_Macs_with_Metal_Support_v1.0.md`](Whisper_on_Intel_Macs_with_Metal_Support_v1.0.md)

### 📦 Version  
**v1.0 – November 2025**  
Compiled by Retroniks

### 📜 License  
MIT License – see `LICENSE`
