
# 🔩 Predicting Maximum von Mises Stress using Deep Learning (CNN & UNet)  
**Course:** ME504 – Deep Learning  
**Project Topic:** Q9 – Structural Analysis using CNN and UNet  
**Team Members:** Prabhreet Kaur, Adrika Sikidar  
**Supervisor:** Dr. Manish Agrawal

---

## 📌 Problem Statement (Q9)

We aim to **predict the maximum von Mises stress** for a given **volume fraction distribution** of a two-material composite domain arranged in a 21x21 grid.

We compare two deep learning approaches:
- **Approach-1:** Use a **CNN** to directly predict the scalar maximum von Mises stress.
- **Approach-2:** Use a **UNet** to predict the **complete von Mises stress field**, from which the maximum value is extracted.

---

## 🧠 Dataset Description

- **Input:** 21×21 grid representing volume fractions (reshaped from 1D vector of 441 values)
- **Target (Approach 1):** Scalar max von Mises stress
- **Target (Approach 2):** Full 21×21 von Mises stress field
- **Samples:** 10,000
- **Normalization:** Applied using mean and standard deviation of training set

---

## ⚙️ Approach-1: CNN

### Architecture:
- Input shape: `(21, 21, 1)`
- 3 Convolutional blocks + MaxPooling
- Flatten → Fully Connected Layers
- Output: Scalar (Maximum von Mises Stress)

### Layers:
- Conv2D (32, 64, 128 filters) with ReLU
- MaxPooling layers for spatial reduction
- Dense layers (64 → 32 → 1)
- **Loss:** MSE  
- **Optimizer:** Adam  
- **Metric:** R² score  

### Performance:
- Efficient and lightweight
- Direct regression avoids field prediction
- Less memory required

---

## 🔁 Approach-2: UNet

### Architecture:
- Input shape: `(21, 21, 1)`
- Encoder: 3 Downsampling blocks
- Bottleneck: Conv2D(128)
- Decoder: 3 Upsampling blocks with skip connections
- Output: Full von Mises stress field (21×21)
- Extracted **maximum τ_vm** from predicted field

### Layers:
- Conv2D (16 → 128) with BatchNorm + ReLU
- Conv2DTranspose for upsampling with cropping
- Skip connections for better spatial learning
- Final Output: 21×21 von Mises field

### Challenges:
- Output size mismatch due to pooling/upsampling
- Tensor shape alignment issues in decoder
- Higher training time and complexity

---

## 📊 Comparison: CNN vs UNet

| Feature              | CNN (Approach-1)         | UNet (Approach-2)                     |
|----------------------|--------------------------|----------------------------------------|
| **Input**            | Volume fraction (21×21)  | Volume fraction (21×21)                |
| **Output**           | Max stress (scalar)      | Full field (21×21), then extract max   |
| **Complexity**       | Low                      | High                                   |
| **Data Requirement** | Moderate                 | Higher (predicts full field)           |
| **Training Time**    | Fast                     | Slower                                 |
| **Ease of Use**      | High                     | Medium – needs tensor shape handling   |
| **Precision**        | High (direct)            | Good, but suffers due to upsampling    |

---

## 🚀 How to Run

1. Clone the repo:
```bash
git clone https://github.com/your-username/von-mises-stress-prediction.git
cd von-mises-stress-prediction

