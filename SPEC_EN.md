# Universal Local AI Benchmark & Recommendation System – Specification

## 1. Vision
Create a free, open‑source, community platform that lets any user automatically determine which local AI models best fit their machine, even on old or modest hardware.

## 2. Main Objectives
- **Automatic hardware detection** – analyse CPU (architecture, cores, threads, real frequency, instruction sets), RAM (total, free, available, swap), GPU (presence, VRAM), storage, OS, kernel version, temperature, power draw (if available).
- **AI model discovery** – query public catalogs (Ollama, Hugging Face, llama.cpp, LM Studio, GPT4All, OpenRouter, KoboldCPP, vLLM, MLX, etc.) to fetch metadata, licences, sizes, estimated RAM/CPU/GPU needs.
- **Intelligent selection** – offer several search strategies:
  - Quick mode: best immediately usable model.
  - Balanced mode: best quality‑speed‑memory trade‑off.
  - Max‑quality mode: best possible model.
  - Experimental mode: test every compatible model.
  - Community mode: test only models recommended by the community.
- **Automatic download** – download, verify integrity, measure real size, and install the chosen model.
- **Automatic benchmarking** – for each model measure:
  - RAM/Swap consumption, CPU/GPU load, temperature, load time.
  - Time to first token, tokens per second, total response time, max context length, image/document/audio processing time (if applicable).
- **Qualitative evaluation** – standardized test suite covering reasoning (logic, maths, deduction), programming (Python, C, Bash, JavaScript), languages (French, English, multilingual), general knowledge (history, science, IT), document summarisation, file analysis, vision (if applicable).
- **Scoring system** – each model receives quality, speed, memory, energy, compatibility and community scores; a weighted global score is computed.
- **Automatic model management** – according to user preferences (keep, archive, delete, keep Top N, etc.).
- **Community database** – allow anonymous sharing of hardware configuration, benchmark results, scores, memory usage, observed speed; no personal data collected.
- **Collective intelligence** – with enough participants, answer queries such as “For an Intel i3‑2310M with 16 GB RAM on Linux, the best performing models are …”.
- **World rankings** – produce tops by category (legacy CPU, modern CPU, Raspberry Pi, Mini‑PC, NVIDIA/AMD/Intel GPU, limited RAM, low power consumption, etc.).
- **Long‑term goal** – become the largest open‑source observatory of real‑world local AI model performance; stay free, open‑source, transparent, community‑driven, cross‑platform and independent of commercial vendors.

## 3. Detailed Features

### 3.1 Hardware Detection
- Run system commands (`lscpu`, `dmidecode`, `lsblk`, `free`, `nvidia‑smi`, `rocminfo`, `glxinfo`, `vcgencmd`, …) or use cross‑platform libraries (psutil, GPUtil, cpuinfo, …).
- Normalise values (real frequency vs. base frequency, available RAM vs. total, …).
- Export to JSON for consumption by other modules.

### 3.2 Model Catalogue
- Dedicated connectors per source (REST API, GraphQL, light scraping, Git repos, …).
- Filter out proprietary/paid models unless the user explicitly enables them.
- Cache metadata (name, size, licence, architecture, quantisation, download URL).
- Allow adding/removing sources via a configuration file.

### 3.3 Selection & Strategy
- Strategy module receiving hardware profile and returning an ordered list of models to test according to the chosen mode.
- Allow custom thresholds (e.g., max RAM, max response time, max energy consumption).

### 3.4 Download & Integrity
- Parallel download with resumability (curl/wget or HTTP library).
- Verify checksum (SHA256) when provided.
- Extract archives (.tar, .zip) and place models in a local store (`~/.local-ai-benchmark/models/`).

### 3.5 Benchmark
- Launch model via its appropriate inference engine (ollama run, llama.cpp, …).
- Warm‑up with a few inferences to stabilise CPU/GPU frequencies.
- Precise timing (`time.perf_counter`) and resource monitoring with psutil during inference.
- Produce a detailed report per model (JSON + optional markdown).

### 3.6 Qualitative Evaluation
- Pre‑defined question/answer sets stored in a `tests/` directory.
- Evaluation script calls the model, gathers answers, compares to references (exactness, pertinence) using simple metrics (accuracy, rough BLEU, etc.).
- Enable community contribution of new test suites.

### 3.7 Scoring & Aggregation
- Normalised scoring function (0‑100) for each criterion.
- Global score = weighted sum (default weights: quality 30 %, speed 25 %, memory 20 %, energy 15 %, community 10 %).
- Incremental update of rankings when a new result is submitted.

### 3.8 Community Backend
- Simple endpoint (or JSON‑Lines files) accepting anonymous payloads like:
  ```json
  {
    "cpu": "...",
    "ram_total_gb": ...,
    "gpu": "...",
    "os": "...",
    "model": "...",
    "tokens_per_sec": ...,
    "latency_ms": ...,
    "mem_used_mb": ...
  }
  ```
- Aggregation service (static GitHub Pages or tiny server) computes means, medians, standard deviations per hardware profile.

### 3.9 Rankings & Recommendations
- Periodically regenerated markdown tables (via cron or GitHub Actions).
- Recommendation endpoint that, given a hardware profile, returns the Top N models from the community database.

## 4. Non‑Functional Constraints
- **Licence** – Open source (MIT or Apache‑2.0).
- **Free‑only** – By default only free / open‑source models and tools are used; no paid model or service is called without explicit user consent.
- **Cross‑platform** – Linux (main distros), Windows, macOS, Raspberry Pi (ARM), other SBCs.
- **Privacy** – No personal data collected or transmitted without explicit anonymisation.
- **Extensibility** – Plugin‑style architecture to easily add new model sources, new benchmarks or new inference engines.
- **Performance** – Detector and scheduler must be lightweight (< 50 MB RAM, < 2 s start‑up) to avoid biasing measurements.

## 5. Deliverables
- Source code hosted on GitHub (repo `universal-local-ai-benchmark`).
- Documentation (`README.md`, `CONTRIBUTING.md`, licence).
- Installation scripts (shell script or `make`).
- Packages for major platforms (AppImage, .exe, Homebrew, Chocolatey, …).
- Community dashboard (GitHub Pages or similar) showing rankings and allowing result submission.
- Basic CI/CD (GitHub Actions) verifying hardware detector and a tiny model benchmark (e.g., `TinyLlama` or `phi‑2`) on every push.

## 6. Indicative Roadmap (phases)
| Phase | Goal | Estimated time |
|-------|------|----------------|
| 0 – Init | Repo, licence, README, .gitignore | 1 wk |
| 1 – Hardware detection | HW module outputting JSON | 2 wks |
| 2 – Model catalogue | Connectors for Ollama, HF, llama.cpp + cache | 3 wks |
| 3 – Selection strategies | Quick/balanced/max‑quality modes | 2 wks |
| 4 – Download & integrity | Resumable download, SHA check | 2 wks |
| 5 – Basic benchmark | Launch a simple model, measure tokens/s | 3 wks |
| 6 – Qualitative tests | Reasoning & programming test suite | 3 wks |
| 7 – Scoring system | Scores, weighted aggregate, config | 2 wks |
| 8 – Community DB | Submission API, simple aggregation | 3 wks |
| 9 – Rankings & recs | Generate tables, recommendation endpoint | 2 wks |
|10 – Packaging & CI/CD | Multiplatform builds, automated tests | 2 wks |
|11 – Docs & community outreach | Guides, tutorials, call for contributors | ongoing |

**Total** ≈ 6 months for a usable first version, followed by iterative improvements driven by community feedback.

---

*This specification is the reference for developing the Universal Local AI Benchmark & Recommendation System. Any evolution must comply with it or be updated via a community‑approved change.* 