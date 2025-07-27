
FROM python:3.10-slim AS builder

# system dependencies 
RUN apt-get update && apt-get install -y \
    build-essential \
    gcc \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir PyMuPDF==1.23.26

# Final runtime stage
FROM python:3.10-slim

WORKDIR /app

RUN mkdir -p /app/input /app/output

COPY --from=builder /usr/local/lib/python3.10/site-packages /usr/local/lib/python3.10/site-packages

COPY process_pdfs.py .

ENV PYTHONUNBUFFERED=1
ENV PYTHONDONTWRITEBYTECODE=1

# Run the script with optimizations
CMD ["python", "-O", "process_pdfs.py"]
