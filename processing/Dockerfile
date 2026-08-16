FROM python:3.10-slim

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

WORKDIR /app

# System deps for PDF/Tesseract
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    tesseract-ocr \
    libtesseract-dev \
    libleptonica-dev \
    poppler-utils \
    wget \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements and install
COPY requirements.txt ./requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Install spaCy model during build for faster startup
RUN python -m spacy download en_core_web_sm || true

# Copy app
COPY . .

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
