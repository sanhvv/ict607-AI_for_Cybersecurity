
# ict607-AI_for_Cybersecurity

# Network Intrusion Detection System - DoS Attack Classification

A machine learning project for detecting Denial-of-Service (DoS) attacks in network traffic using the UNSW-NB15 dataset. This project implements and compares multiple classification models to identify and classify various types of network attacks.

## Project Overview

This project develops and evaluates machine learning models to detect and classify different types of network intrusions, with a focus on Denial-of-Service (DoS) attacks. The system uses the UNSW-NB15 dataset, which contains labeled network traffic flows for both normal and attack scenarios.

## Dataset

### UNSW-NB15 Dataset
- **Source**: Kaggle - UNSW-NB15 Network Intrusion Detection Dataset
- **Description**: Comprehensive dataset containing network traffic records with attack labels
- **Files**: 4 CSV files (UNSW-NB15_1.csv through UNSW-NB15_4.csv)
- **Size**: ~2.5 million records after combination
- **Features**: 42+ network traffic features
- **Target Classes**: 9 attack categories + normal traffic

### Attack Categories
1. **DoS (Denial of Service)** - Attempts to make a service unavailable
2. **Backdoor** - Unauthorized remote access
3. **Exploits** - Taking advantage of software vulnerabilities
4. **Generic** - Generic attacks (includes shellcode)
5. **Reconnaissance** - Information gathering (probing, scanning)
6. **Worms** - Self-replicating malware
7. **Analysis** - Unusual protocol behavior and policy violations
8. **Normal** - Legitimate network traffic

## Project Workflow

### 1. Data Acquisition & Preparation
- Download UNSW-NB15 dataset from Kaggle using Kaggle API
- Load 4 separate CSV files and combine into single dataset
- Encoding: ISO-8859-1 for proper character handling

### 2. Feature Engineering
Selected 37 relevant network features focusing on:
- **Network Traffic Volume/Rate**: duration, bytes sent/received, packet counts, load
- **Transport Layer Behavior**: TTL values, TCP RTT, SYN/ACK acknowledgments
- **Application/Session Layer**: Protocol, service, state, HTTP methods, FTP commands
- **Behavioral/Statistical**: Mean packet size, jitter, inter-packet time
- **Connection-Based**: Connection statistics over various time windows

### 3. Data Cleaning & Preprocessing
- **Null Value Handling**:
  - `attack_cat`: Filled with 'normal'
  - `ct_flw_http_mthd`: Filled with 0
  - `is_ftp_login`: Filled with 0
  
- **Duplicate Removal**: Removed duplicate records
- **Type Conversion**:
  - `dsport`: Converted to integer
  - `ct_ftp_cmd`: Cleaned and converted to integer
  
- **Data Normalization**: Applied StandardScaler to all features

### 4. Label Encoding
Used LabelEncoder for categorical features:
- `proto`: Network protocol (TCP, UDP, ICMP, etc.)
- `state`: Connection state (e.g., CON, INT, FIN, RST)
- `service`: Service type (e.g., http, smtp, ftp)
- `attack_cat`: Attack category (target variable)

### 5. Class Balancing
- **Challenge**: Highly imbalanced dataset
- **Solution**: Fixed to 15,000 samples per attack class
- **Method**: Random sampling with stratification
- **Result**: Balanced dataset with equal representation for each class

### 6. Exploratory Data Analysis
- **Correlation Analysis**: Heatmap showing feature relationships
- **Attack Distribution**: Pie chart showing attack type distribution
- **Class Balance Verification**: Confirming equal samples per class

### 7. Model Development
Implemented and compared three machine learning models:

#### Random Forest Classifier
- **Algorithm**: Ensemble of decision trees
- **Configuration**: 100 estimators
- **Strengths**: Handles non-linear relationships, feature importance analysis
- **Use Case**: Baseline model, interpretability

#### Support Vector Machine (SVM)
- **Algorithm**: Support Vector Machine
- **Kernel**: RBF (Radial Basis Function)
- **Strengths**: Effective in high-dimensional spaces
- **Use Case**: Complex decision boundaries

#### XGBoost Classifier
- **Algorithm**: Extreme Gradient Boosting
- **Objective**: Multi-class classification (softmax)
- **Metric**: Multi-class log loss
- **Strengths**: State-of-the-art performance, handles class imbalance
- **Use Case**: Best performing model

### 8. Model Evaluation
Metrics calculated for each model:
- **Accuracy**: Overall correctness
- **Precision**: True positives / (True positives + False positives)
- **Recall**: True positives / (True positives + False negatives)
- **F1-Score**: Harmonic mean of precision and recall
- **Confusion Matrix**: Detailed per-class prediction breakdown

### 9. Results & Visualization
- **Accuracy Comparison**: Bar chart comparing overall accuracy
- **Per-Class Metrics**: Precision, Recall, F1-Score by attack type
- **DoS-Specific Analysis**: Focused metrics for DoS attack detection
- **Confusion Matrices**: Heatmaps for each model showing prediction patterns

## Key Features

- **Multi-Model Comparison**: Side-by-side evaluation of RF, SVM, and XGBoost
- **Balanced Dataset Handling**: Class balancing to prevent bias
- **Comprehensive Visualization**: Heatmaps, bar charts, and distribution plots
- **Feature Scalability**: StandardScaler for normalized feature values
- **Train-Test Split**: Stratified 70-30 split ensuring class distribution

## Data Structure

### Input Features (34 numerical + encoded features):
```
- Network Layer: dur, sbytes, dbytes, Spkts, Dpkts
- Transport Layer: sttl, dttl, tcprtt, synack, ackdat
- Connection Stats: is_sm_ips_ports, sloss, dloss, dsport
- Service: service_encoded, state_encoded, proto_encoded, ct_flw_http_mthd
- Application: is_ftp_login, ct_ftp_cmd, res_bdy_len
- Behavioral: smeansz, dmeansz, trans_depth, Sjit, Djit, Sintpkt, Dintpkt
- Connection-Based: ct_srv_dst, ct_dst_ltm, ct_src_dport_ltm, ct_dst_sport_ltm, ct_dst_src_ltm
```

### Target Variable:
- `attack_type_encoded`: Encoded attack category (0-7)

## Requirements

```
pandas>=1.0.0
numpy>=1.18.0
scikit-learn>=0.22.0
xgboost>=1.0.0
matplotlib>=3.0.0
seaborn>=0.10.0
kaggle>=1.5.0
```

## Installation

### Local Setup
```bash
# Clone or download the project
cd ICT607_Project

# Create virtual environment (optional)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### Google Colab Setup
The notebook is optimized for Google Colab:
```python
# Install Kaggle
!pip install kaggle

# Mount Google Drive
from google.colab import drive
drive.mount('/content/drive')

# Configure Kaggle API
!mkdir -p ~/.kaggle
!cp /content/drive/MyDrive/kaggle/kaggle.json ~/.kaggle/
!chmod 600 ~/.kaggle/kaggle.json

# Download dataset
!kaggle datasets download -d mrwellsdavid/unsw-nb15
!unzip unsw-nb15.zip
```

## Usage

### Running in Google Colab (Recommended)
```bash
# Open the notebook in Google Colab
# Run all cells sequentially
```

### Running Locally
```python
# The notebook can be adapted for local execution
jupyter notebook code.ipynb
```

### Key Execution Steps
1. **Setup**: Install dependencies and configure Kaggle API
2. **Data Loading**: Download and load UNSW-NB15 dataset
3. **Preprocessing**: Clean, balance, and prepare data
4. **Encoding**: Apply label encoding to categorical features
5. **Scaling**: Normalize features using StandardScaler
6. **Training**: Train Random Forest, SVM, and XGBoost models
7. **Evaluation**: Generate classification reports and confusion matrices
8. **Comparison**: Visualize model performance across metrics

## Project Files

```
ICT607/Project/
├── code.ipynb                           # Main Jupyter notebook
├── README.md                            # This file
├── 607 Project.docx                    # Project requirements
├── Project Proposal V1.docx            # Project proposal
├── Project Proposal_Group 16_Van Sanh VU_34900798.docx
├── Group 16 - Project PPT.pptx         # Presentation slides
├── Project.pdf                         # Additional documentation
├── UNSW-NB15_features.csv              # Feature definitions (downloaded)
├── UNSW-NB15_1.csv through UNSW-NB15_4.csv  # Raw data (downloaded)
└── cleaned_df_new                      # Preprocessed dataset (output)
```

## Model Performance Summary

Each model provides:
- Individual class metrics (Precision, Recall, F1-Score)
- Overall accuracy score
- Confusion matrix visualization
- Comparative analysis across all three models

### Expected Performance Characteristics:
- **Random Forest**: Good baseline, interpretable results
- **SVM**: Competitive performance, computationally intensive
- **XGBoost**: Typically best overall performance with balanced metrics

## Attack Detection Capabilities

The system can detect:
- ✓ Denial-of-Service (DoS) attacks
- ✓ Backdoor attacks
- ✓ Exploit attempts
- ✓ Generic attacks
- ✓ Reconnaissance activities
- ✓ Worms and malware
- ✓ Policy violations
- ✓ Normal traffic

## Visualizations Generated

1. **Correlation Heatmap**: Feature correlations in the balanced dataset
2. **Attack Distribution Pie Chart**: Distribution of attack types (excluding normal)
3. **Confusion Matrices**: For each model (Random Forest, SVM, XGBoost)
4. **Accuracy Comparison**: Bar chart comparing overall model accuracy
5. **Per-Class Metrics**: Precision, Recall, F1-Score by attack category
6. **DoS-Specific Comparison**: Focused metrics for DoS attack detection

## Key Findings

- Dataset contains balanced representation after sampling
- Different models show varying performance across attack categories
- Some attack types are more easily detected than others
- Feature scaling improves model performance significantly
- Ensemble methods (Random Forest, XGBoost) show competitive results

## Future Enhancements

1. **Deep Learning**: Implement neural networks (CNN, LSTM, RNN)
2. **Feature Importance**: Analyze which features are most important for detection
3. **Hyperparameter Tuning**: GridSearchCV for optimal parameters
4. **Cross-Validation**: K-fold validation for more robust evaluation
5. **Real-time Detection**: Deploy model for live network monitoring
6. **Anomaly Detection**: Unsupervised learning approaches
7. **Ensemble Voting**: Combine predictions from multiple models
8. **Class Weighting**: Alternative to balancing for imbalanced data
9. **Feature Selection**: PCA or correlation-based feature reduction
10. **Model Deployment**: Flask/FastAPI for API-based detection service

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Kaggle API not found | Run `pip install kaggle` and configure credentials |
| Memory errors | Reduce samples_per_class in balancing step |
| Missing features | Ensure all 4 UNSW-NB15 CSV files are downloaded |
| Encoding errors | Use ISO-8859-1 encoding when reading CSV files |
| Feature not found | Verify feature names in dos_features list |

## Academic Information

- **Course**: ICT607 - Advanced Data Science
- **Institution**: Murdoch University
- **Semester**: S1 2025
- **Group**: Group 16
- **Student**: Van Sanh VU
- **Student ID**: 34900798

## References

- **UNSW-NB15 Dataset**: Moustafa, N., Slay, J. (2015). UNSW-NB15: A comprehensive data set for network intrusion detection systems
- **Kaggle**: https://www.kaggle.com/datasets/mrwellsdavid/unsw-nb15
- **Scikit-learn**: https://scikit-learn.org/
- **XGBoost**: https://xgboost.readthedocs.io/

## License

This project is for educational purposes as part of the ICT607 course at Murdoch University.

---

For questions or issues, please refer to the course materials or contact the course instructor.
>>>>>>> 56a9513 (init)
