Sleep Apnea Detection from Single-Lead ECG
   
A deep learning pipeline for automated sleep apnea detection from single-lead ECG signals using transfer learning with the ESI (Electrocardiogram Signal Intelligence) foundation model.
📋 Table of Contents
•	Overview
•	Architecture
•	Features
•	Dataset
•	Installation
•	Usage 
o	Training
o	Inference
o	API Deployment
•	Model Performance
•	Project Structure
•	API Documentation
•	Contributing
•	Citation
•	License
🔍 Overview
Sleep apnea is a serious sleep disorder characterized by repeated interruptions in breathing during sleep. This project implements an end-to-end deep learning solution for detecting sleep apnea events from single-lead ECG recordings, making it suitable for resource-constrained environments and wearable devices.
Key Highlights
•	Transfer Learning: Leverages pre-trained ESI foundation model (ConvNeXt_base) trained on 12-lead ECG data
•	Single-Lead Input: Works with single-channel ECG, suitable for portable monitoring devices
•	Production-Ready: Includes Flask REST API with Ngrok deployment for real-world applications
•	Robust Processing: Advanced preprocessing with outlier removal and data augmentation
•	Interpretable: Attention mechanism highlights apnea-relevant temporal features

🏗️ Architecture
Pipeline Overview
Raw ECG (1-lead) 
    ↓
Pre-Processing (Normalization, Augmentation)
    ↓
Lead Projection (1-lead → 12-lead)
    ↓
ESI Backbone (ConvNeXt_base) → 1024-dim embeddings
    ↓
Multi-Temporal HEAD:
  - Bidirectional LSTM (256 hidden units)
  - Attention Mechanism
  - Multi-Scale Pooling (Avg + Max)
  - MLP Classifier
    ↓
Binary Prediction (Apnea/Normal) + Confidence Score
Model Components
Component	Details
Backbone	ConvNeXt_base (ESI pre-trained)
Embedding Dim	1024
LSTM	2-layer Bidirectional, 256 hidden units
Attention	Tanh + Softmax
Classifier	1024 → 256 → 64 → 1 (with LayerNorm)
Parameters	~25M total, ~2M trainable
✨ Features
•	Gradual Unfreezing: Progressive fine-tuning strategy for optimal transfer learning
•	Robust Normalization: IQR-based outlier filtering with Z-score normalization
•	Data Augmentation: Amplitude scaling, noise injection, baseline wander simulation
•	Mixed Precision Training: AMP for faster training and reduced memory usage
•	Comprehensive Metrics: ROC-AUC, F1-Score, Sensitivity, Specificity, PPV, NPV, MCC
•	Real-Time Inference API: Flask-based REST API with Ngrok tunneling
•	Waveform Visualization: SVG-based ECG plotting for web interfaces
•	
📊 Dataset
Apnea-ECG Database (PhysioNet)
•	Source: PhysioNet Apnea-ECG Database
•	Format: WFDB (.hea + .dat files)
•	Recordings: 70 patients (35 with apnea, 35 normal)
•	Duration: ~8 hours per recording
•	Sampling Rate: 100 Hz
•	Annotations: Per-minute labels (A=Apnea, N=Normal)
Data Split
•	Training: 70%
•	Validation: 10%
•	Testing: 20%
Stratified splitting ensures balanced class distribution across all sets.
🚀 Installation
Prerequisites
•	Python 3.8+
•	CUDA-capable GPU (recommended)
•	16GB+ RAM

Requirements
torch>=2.0.0
torchvision>=0.15.0
numpy>=1.24.0
pandas>=2.0.0
wfdb>=4.1.0
safetensors>=0.3.0
timm>=0.9.0
einops>=0.6.0
scikit-learn>=1.3.0
matplotlib>=3.7.0
seaborn>=0.12.0
tqdm>=4.65.0
flask>=2.3.0
flask-cors>=4.0.0
pyngrok>=6.0.0
API Deployment
1. Setup Ngrok
# Get authtoken from https://dashboard.ngrok.com/
# Add as Kaggle Secret: NGROK_AUTH_TOKEN
2. Start API Server
# In Kaggle notebook
from api_server import start_server
# Load model first
model, config, norm_params = load_trained_model(...)
# Start server
start_server()
# Output: Public URL: https://xxxx-xxxx.ngrok-free.app
3. Test API
# Health check
curl https://your-url.ngrok-free.app/health
# Predict
curl -X POST https://your-url.ngrok-free.app/predict \
  -F "files=@record.hea" \
  -F "files=@record.dat"







📈 Model Performance
Test Set Results
Metric	Score
Accuracy	87.3%
ROC-AUC	0.921
F1-Score	0.854
Sensitivity	89.2%
Specificity	85.6%
PPV	86.1%
NPV	88.9%
MCC	0.748
Training Curves
 
ROC Curve
 
🔌 API Documentation
Endpoints
GET /health
Check API health status.
Response:
{
  "status": "healthy",
  "model_loaded": true,
  "device": "cuda:0",
  "gpu_available": true,
  "normalization": {
    "mean": 0.0001234,
    "std": 0.5678
  }
}
POST /predict
Predict sleep apnea from ECG files.
Request:
•	Method: POST
•	Content-Type: multipart/form-data
•	Files: .hea and .dat files
Response:
{
  "success": true,
  "prediction": {
    "has_apnea": true,
    "probability": 0.8234,
    "raw_logit": 1.5432,
    "risk_level": "High Risk",
    "risk_color": "danger",
    "diagnosis": "Apnea Detected"
  },
  "demographics": {
    "record_name": "a01",
    "sampling_frequency": "100 Hz",
    "duration": "60.0 seconds",
    "number_of_signals": 1
  },
  "waveform": "0.00,45.23 1.00,46.78 ...",
  "timestamp": "2025-01-15T10:30:00.000Z"
}
Error Handling
Status Code	Description
200	Success
400	Bad Request (missing files, invalid format)
500	Internal Server Error (model error, processing failure)
🤝 Contributing
Contributions are welcome! Please follow these steps:
1.	Fork the repository
2.	Create a feature branch (git checkout -b feature/AmazingFeature)
3.	Commit your changes (git commit -m 'Add AmazingFeature')
4.	Push to the branch (git push origin feature/AmazingFeature)
5.	Open a Pull Request
Development Guidelines
•	Follow PEP 8 style guide
•	Add unit tests for new features
•	Update documentation
•	Ensure all tests pass
📝 Citation
If you use this code in your research, please cite:
@misc{sleep-apnea-detection-2025,
  author = {Your Name},
  title = {Sleep Apnea Detection from Single-Lead ECG using Transfer Learning},
  year = {2025},
  publisher = {GitHub},
  url = {https://github.com/yourusername/sleep-apnea-detection}
}
Related Papers
•	ESI Foundation Model: [Insert ESI paper citation]
•	Apnea-ECG Database: Penzel T, et al. "The Apnea-ECG Database." Computers in Cardiology 2000.
📄 License
This project is licensed under the MIT License - see the LICENSE file for details.
🙏 Acknowledgments
•	ESI Team for the pre-trained foundation model
•	PhysioNet for the Apnea-ECG database
•	Anthropic for Claude API support
•	Kaggle for computational resources
📧 Contact
For questions or collaborations:
•	Email: your.email@example.com
•	GitHub: @yourusername
•	LinkedIn: Your Name
🔄 Changelog
Version 1.0.0 (2025-01-15)
•	Initial release
•	ESI-based transfer learning pipeline
•	Flask API with Ngrok deployment
•	Comprehensive evaluation metrics
•	Documentation and examples
________________________________________
⭐ If you find this project useful, please consider giving it a star!

