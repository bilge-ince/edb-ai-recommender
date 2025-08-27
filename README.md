# Intelligent Shopping Experience

An intelligent shopping experience showcasing AI-powered product recommendations using **OpenShift AI** and **EDB Postgres AI**. The application demonstrates advanced machine learning capabilities including multimodal search, vector embeddings, and AI-generated content summarization powered by OpenShift AI's Elyra pipelines and model serving, with EDB Postgres AI's AIDB extension for vector operations. It enhances the user shopping experience by presenting similar items and providing summarized insights from user reviews.

## 🌟 Features

- **Multimodal Search**: Search products using both text descriptions and uploaded images
- **AI-Powered Recommendations**: Leverage CLIP and GritLM models for intelligent product matching  
- **Smart Filtering**: Filter results by gender, category, and other product attributes
- **Review Summarization**: AI-generated summaries and labels from customer reviews using Llama-3.1-8B
- **Vector Similarity**: Advanced embedding-based search without leaving the database using EDB AIDB extension
- **Cloud Storage Integration**: S3-compatible storage for product images
- **Kubernetes Ready**: Complete Helm charts for production deployment

## 🏗️ Architecture

### Core Components

- **Frontend**: Streamlit web application with intuitive search interface
- **Database**: EDB Postgres AI with AIDB extension for vector operations
- **ML Pipeline**: OpenShift AI Elyra notebooks for data processing and model deployment
- **Storage**: S3-compatible object storage for product images
- **Inference**: OpenShift AI model serving runtime for GritLM and Llama inference

### AI Models (served via OpenShift AI)

- **CLIP (Multimodal)**: Text and image embeddings for similarity search as a local model in AIDB.
- **GritLM-7B**: Text embeddings for product descriptions (OpenShift AI model serving)
- **Llama-3.1-8B**: Generative AI for review summarization (OpenShift AI model serving)

## 🚀 Quick Start

### Prerequisites

- Python 3.8+
- EDB Postgres AI AIDB extension 4.1+
- OpenShift AI (for Elyra pipelines and model serving)
- S3-compatible storage (AWS S3, MinIO, etc.)
- Docker/Podman (for containerized deployment)

### Local Development

1. **Clone the repository**
   ```bash
   git clone https://github.com/RHEcosystemAppEng/edb-ai-recommender
   cd edb-ai-recommender
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Set environment variables**
   ```bash
   # Database configuration
   export EDB_AIDB_PORT="postgresql://host:port"
   export DATABASE_USER="your_username"
   export DATABASE_PASSWORD="your_password"
   export DATABASE_NAME="recommender_db"
   
   # S3 storage configuration
   export S3_ENDPOINT_URL="https://your-s3-endpoint"
   export S3_BUCKET_NAME="your-bucket"
   export S3_REGION="your-region"
   export S3_ACCESS_KEY="your-access-key"
   export S3_SECRET_KEY="your-secret-key"
   export S3_RECOMMENDER_IMAGES_PATH="images/"
   ```

4. **Initialize the database and load data**
   
   **Option A: Using OpenShift AI Elyra Pipeline**
   ```bash
   # Trigger the complete initialization pipeline through Elyra
   # This runs the notebooks in the elyra_pipeline/ directory in sequence:
   # 1. prepare_schema.ipynb - Database schema setup
   # 2. populate_catalog.ipynb - Load product data
   # 3. upload_images.ipynb - Upload images to S3
   # 4. create_extensions.ipynb - Setup AIDB extensions
   # 5. create_retrievers.ipynb - Create AI models and knowledge bases
   # 6. compute_text_embeddings.ipynb - Generate text embeddings
   # 7. compute_image_embeddings.ipynb - Generate image embeddings
   
   # Execute via Elyra Pipeline (requires OpenShift AI environment)
   elyra-pipeline run init-recommender.pipeline
   ```

   **Option B: Direct Python execution**
   ```bash
   python src/connect_encode.py
   ```

5. **Run the application**
   ```bash
   streamlit run app_search_aidb.py
   ```

## 🐳 Container Deployment

### Build and Run Locally
```bash
# Build container
podman build -t ai-recommender .

# Run container
podman run -p 8501:8501 \
  -e DATABASE_USER=your_user \
  -e DATABASE_PASSWORD=your_password \
  # ... other environment variables
  ai-recommender
```

### Deploy to Container Registry
```bash
./deploy-web-app.sh
```

## ☸️ Kubernetes Deployment

The application includes comprehensive Helm charts for Kubernetes deployment:

### Deploy All Components
```bash
# Deploy workbench environment
./deploy-all.sh

# Or deploy individual components
helm install ai-recommender ./kubernetes/ai-recommender
helm install edb-aidb ./kubernetes/edb-aidb
helm install inference ./kubernetes/inference
```

### Available Charts
- `ai-recommender`: Main application deployment
- `edb-aidb`: EDB Postgres AI database with AIDB extension
- `inference`: ML model serving runtime
- `workbench`: Development environment with Jupyter notebooks
- `common`: Shared configurations and secrets

## 📊 Data Pipeline

### OpenShift AI Elyra Notebooks

The `elyra_pipeline/` directory contains Jupyter notebooks for OpenShift AI Elyra pipeline execution:

- **Data Preparation**: `prepare_schema.ipynb`, `populate_catalog.ipynb`
- **Image Processing**: `resize_images.ipynb`, `upload_images.ipynb`
- **Embeddings**: `compute_text_embeddings.ipynb`, `compute_image_embeddings.ipynb`
- **Model Setup**: `create_extensions.ipynb`, `create_retrievers.ipynb`

### Pipeline Execution
```bash
# Run complete initialization pipeline
python src/connect_encode.py
```

## 🔍 Usage

### Text Search
1. Enter a product description (e.g., "red summer dress")
2. Optionally filter by gender
3. Click "Search with Text"

### Image Search
1. Upload a product image
2. Optionally apply gender filters
3. Click "Search with Image"

### Product Reviews
- Click "Review" on any search result
- View AI-generated review summaries and sentiment labels
- Browse individual customer reviews

## 🛠️ Development

### Project Structure
```
edb-ai-recommender/
├── app_search_aidb.py          # Main Streamlit application
├── pages/review_page.py        # Product review page
├── src/                        # Core application modules
│   ├── db_connection.py        # Database connectivity
│   ├── s3_connection.py        # S3 storage integration
│   └── connect_encode.py       # Data pipeline and ML setup
├── elyra_pipeline/            # Jupyter notebooks for data processing
├── kubernetes/                # Helm charts for deployment
├── dataset/                   # Sample data files
└── requirements.txt           # Python dependencies
```

### Key Dependencies
- `streamlit`: Web application framework
- `psycopg2-binary`: PostgreSQL adapter
- `sqlalchemy`: Database ORM
- `boto3`: AWS SDK for S3 operations
- `pillow`: Image processing
- `pandas`: Data manipulation
- `numpy`: Numerical computing

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 🆘 Support

For questions and support, please open an issue in the GitHub repository.

---

**Built with ❤️ through Red Hat and EDB collaboration**  
**Powered by OpenShift AI and EDB Postgres AI**
