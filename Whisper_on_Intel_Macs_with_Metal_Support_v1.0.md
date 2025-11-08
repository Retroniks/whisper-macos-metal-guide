# Whisper on Intel Macs with Metal Support  
**Compiled by Retroniks**  

*Tested on macOS Sequoia 15.7.2 (OCLP) with AMD Radeon RX 580 (8 GB, Metal 2 supported).*  

---

## ⚙️ Overview  
This document provides a concise, practical step-by-step guide to build and run **whisper.cpp** on Intel-based macOS systems that expose Metal through an AMD GPU (RX 580 etc.).  
It focuses on reproducibility: installing Homebrew, dependencies, CMake build, model download / quantization and a working transcription example.  
All paths are generic so the guide is safe to publish.  

---

## 💾 Requirements  
- Intel macOS machine with Metal-capable GPU (tested with RX 580)  
- macOS with functional Metal drivers (Sequoia + OCLP tested)  
- Internet connection for initial installs and model download  
- ~2–8 GB free disk space (depends on model size)  

---

## 0️⃣ Quick notes  
- Build via CMake; run the native **whisper.cpp** executable — no Core ML steps.  
- If Homebrew already exists, skip its install.  
- The resulting binary lives in `build/bin/`.  

---

## 1️⃣ Install Xcode Command Line Tools  
Open Terminal and run:  
```bash
xcode-select --install
# macOS will confirm if already installed.
```

---

## 2️⃣ Install Homebrew (if missing) and packages  
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
eval "$(/usr/local/bin/brew shellenv)" || true

brew update
brew install git cmake ffmpeg pandoc tectonic
```
- 🧰 `ffmpeg` = audio conversion  
- 🧱 `cmake`, `git` = build tools  
- 📄 `pandoc`, `tectonic` = optional MD→PDF export  

---

## 3️⃣ Clone the repository  
```bash
cd ~
git clone https://github.com/ggerganov/whisper.cpp.git
cd whisper.cpp
```

---

## 4️⃣ Build (Release mode)  
```bash
cmake -B build
cmake --build build -j --config Release
```
Check the binaries:  
```bash
ls -lah build/bin
```
Use whichever executable appears (`main`, `whisper`, or `whisper-cli`).  

---

## 5️⃣ Download a model  
`medium` offers a solid balance:  
```bash
sh ./models/download-ggml-model.sh medium
# → models/ggml-medium.bin
```
Other sizes: `tiny`, `base`, `small`, `large-v3`, etc.  

---

## 6️⃣ Optional – Quantize (q5_0)  
Reduce RAM & disk usage:  
```bash
./build/bin/quantize ./models/ggml-medium.bin ./models/ggml-medium-q5_0.bin q5_0
```

---

## 7️⃣ Prepare audio (FFmpeg)  
Convert to mono 16-bit PCM 16 kHz WAV:  
```bash
ffmpeg -i input_audio.m4a -ar 16000 -ac 1 -c:a pcm_s16le input.wav
```

---

## 8️⃣ Run transcription  
Replace `main` if your binary differs:  
```bash
./build/bin/main   -m ./models/ggml-medium-q5_0.bin   -f input.wav   -l en   -t 8   -otxt -of out/transcript
```
**Flags:**  
`-m` model   `-f` input   `-l` lang   `-t` threads   `-otxt -of` output file  

---

## 9️⃣ Troubleshooting  
- 🔄 Rebuild if no binary: `cmake -B build && cmake --build build -j`  
- ⚙️ Permissions: `chmod +x build/bin/*`  
- 🧠 Slow? Check Metal support via `system_profiler SPDisplaysDataType`  
- 🎧 FFmpeg errors? Run `ffmpeg -i input_audio` to inspect codecs.  

---

## 🔧 Export to PDF (optional)  
```bash
pandoc Whisper_on_Intel_Macs_with_Metal_Support_v1.0.md   -o Whisper_on_Intel_Macs_with_Metal_Support_v1.0.pdf   --pdf-engine=tectonic
```

---

## 🗂 Recommended GitHub layout  
```
whisper-macos-metal-guide/
├─ Whisper_on_Intel_Macs_with_Metal_Support_v1.0.md
├─ README.md
├─ LICENSE
└─ releases/
```

Commit message example:  
```
Initial commit: Whisper on Intel Macs with Metal Support v1.0 — Compiled by Retroniks
```

---

## 📜 Attribution & License  
This guide references **whisper.cpp** (ggerganov / whisper.cpp).  
Include an MIT license or equivalent if redistributed.  

---

*Compiled by Retroniks • Version 1.0 • November 2025*
