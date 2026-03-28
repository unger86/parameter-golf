# ⛳ OpenAI Parameter Golf: M4-Optimized 15.6M Model

![Model Size](https://img.shields.io)
![Framework](https://img.shields.io)
![Hardware](https://img.shields.io)
![License](https://img.shields.io)

> **Эффективная языковая модель, специально оптимизированная для работы на Apple Silicon M4. Максимальная плотность знаний при строгом лимите параметров.**

---

### 🏆 Текущие достижения (Latest Benchmarks)


| Metric | Value |
| :--- | :--- |
| **Validation Loss** | `2.3554` (Step 1000) |
| **Bits Per Byte (BPB)** | `1.4109` |
| **Train Loss** | `2.4125` |
| **Token Throughput** | `~13,300 tok/s` |

---

### 🏗 Архитектурные особенности

Для победы в **Parameter Golf** мы отказались от стандартных глубоких и узких сетей в пользу "широкой" архитектуры:

*   **Wide MLP:** Использование `MLP_MULT=3` позволило упаковать больше логических связей в каждый слой.
*   **Dense Layers:** 6 слоев с `MODEL_DIM=528` обеспечивают оптимальный баланс между скоростью вычислений и глубиной контекста.
*   **Efficient Attention:** GQA (Grouped Query Attention) с 4 KV-головами для экономии памяти без потери качества.

---

### ⚙️ Конфигурация обучения


| Hyperparameter | Value |
| :--- | :--- |
| **Optimizer** | Muon (Matrix) + AdamW (Scalar) |
| **Muon Momentum** | `0.9` |
| **Learning Rate** | `0.01` (Matrix) / `0.05` (Embed) |
| **Batch Size** | `524,288 tokens` |
| **Sequence Length** | `1024` |
| **Warmdown** | `3000 steps` (Linear decay) |

---

### 🚀 Быстрый запуск (Quick Start)

Для воспроизведения результата на **Mac mini M4 (16GB+)** выполните команду:

```bash
# Запуск финального обучения
RUN_ID=m4_fixed_16m \
ITERATIONS=10000 \
NUM_LAYERS=6 \
MODEL_DIM=528 \
MLP_MULT=3 \
MUON_MOMENTUM=0.9 \
TRAIN_BATCH_TOKENS=524288 \
GRAD_ACCUM_STEPS=64 \
VAL_LOSS_EVERY=500 \
WARMUP_STEPS=200 \
WARMDOWN_ITERS=3000 \
MATRIX_LR=0.01 \
python3 train_gpt_mlx.py
