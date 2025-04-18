# Use official slim image for Python 3.12 (Debian-based)
FROM python:3.12-slim-bookworm

# Set working directory
WORKDIR /dsbc25/app

# Install system dependencies first (layer caching optimization)
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    curl \
    build-essential && \
    rm -rf /var/lib/apt/lists/*

# Copy requirements first to leverage Docker cache
COPY ./requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . /dsbc25/app

# Run as non-root user
RUN useradd -m appuser && chown -R appuser /dsbc25/app
USER appuser

# Verify Python path
ENV PYTHONPATH=/dsbc25

EXPOSE 8000

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]