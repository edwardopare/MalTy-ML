# Malaria and Typhoid Detection using Machine Learning

This project aims to develop and evaluate machine learning models for the detection of Malaria from blood smear images and Typhoid fever based on clinical symptoms.

---

## Table of Contents

- [About The Project](#about-the-project)
- [Built With](#built-with)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [Usage](#usage)
- [Dataset](#-dataset)
- [Model Architecture](#-model-architecture)
- [Results](#-results)
- [Contributing](#-contributing)
- [License](#-license)
- [Contact](#-contact)

---

## About The Project

Malaria and Typhoid are life-threatening diseases prevalent in many parts of the world. Early and accurate diagnosis is key to effective treatment and preventing mortality. This project explores the use of machine learning to aid in this diagnostic process.

Main components are included:

A classical machine learning model (Logistic Regression, SVM, or a tree-based model) is used to predict the likelihood of both malaria and Typhoid fever based on a set of patient symptoms.

The goal is to provide a fast, accessible, and reliable tool for preliminary diagnosis.

---

## Built With

This project is built with Python and several key data science libraries:

*   [Python](https://www.python.org/)
*   [Scikit-learn](https://scikit-learn.org/)
*   [Pandas](https://pandas.pydata.org/)
*   [NumPy](https://numpy.org/)
*   [Matplotlib](https://matplotlib.org/)

---

## Getting Started

To get a local copy up and running, follow these simple steps.

### Prerequisites

Make sure you have Python 3.8+ and `pip` installed on your system.

### Installation

1.  **Clone the repository**
    ```sh
    git clone https://github.com/your-username/Malaria-Typhoid-ML.git
    cd Malaria-Typhoid-ML
    ```

2.  **Create and activate a virtual environment** (recommended)
    ```sh
    # For Windows
    python -m venv venv
    .\venv\Scripts\activate

    # For macOS/Linux
    python3 -m venv venv
    source venv/bin/activate
    ```

3.  **Install required packages**
    *(You should create a `requirements.txt` file for your project)*
    ```sh
    pip install tensorflow scikit-learn pandas numpy matplotlib opencv-python
    ```

---
---

## Dataset

Both the malaria and typhoid detection models were trained on synthetically generated datasets based on clinical symptoms.

---
---

## Results

Provide a summary of your model's performance.

**Malaria Model Performance:**
| Metric    | Accuracy | Precision | Recall | F1-Score |
|-----------|----------|-----------|--------|----------|
| **Value** | 96%      | 95%       | 97%    | 96%      |

**Typhoid Model Performance:**
| Metric    | Accuracy | Precision | Recall | F1-Score |
|-----------|----------|-----------|--------|----------|
| **Value** | 88%      | 85%       | 90%    | 87%      |


---

## Contributing

Contributions are what make the open-source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1.  Fork the Project
2.  Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3.  Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4.  Push to the Branch (`git push origin feature/AmazingFeature`)
5.  Open a Pull Request

---

## License

Distributed under the MIT License. See `LICENSE` for more information.

---

## 📫 Contact

* Edward Opare-Yeboah - edwop68@gmail.com
* Prince Acquah Rockson - parockson@gmail.com
* Eric Sena Semordzi - esemordzi001@st.ug.edu.gh


Project Link: https://github.com/edwardopare/MalTy-ML
