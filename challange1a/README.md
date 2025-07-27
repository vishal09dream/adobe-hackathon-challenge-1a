```
challenge1a/
├── app/
│   ├── main.py                  # Entry point for PDF processing pipeline
│   ├── process_pdfs.py          # Core PDF processing and extraction logic
│   ├── utils.py                 # Utility functions and helper methods
│   └── requirements.txt         # Python dependencies
│
├── sample_dataset/
│   └── outputs/                 # Directory for processed results and outputs
│
├── pdfs/                        # Input PDF collection for processing
│   ├── E0CCGSS312.pdf
│   ├── E0H1CM114.pdf
│   ├── Japanesepdf.pdf
│   ├── file01.pdf
│   ├── file02.pdf
│   ├── file03.pdf
│   ├── file04.pdf
│   └── file05.pdf
│
├── Dockerfile                   # Container configuration for deployment
├── README.md                    # Project documentation and setup guide
└── approach_explanation.md      # Technical methodology and implementation details
```
# Adobe Hackathon 2025 – Challenge 1A
**Intelligent PDF Outline Extractor**

**Team Name:** [Your Team Name]  
**Challenge ID:** Round 1A  
**Submission Date:** July 27, 2025

## Overview

This solution addresses the Adobe Hackathon 2025 Challenge 1A by providing an intelligent PDF outline extraction system. Our application automatically analyzes PDF documents and extracts structured outlines, headings, and hierarchical content organization with multi-language support and advanced heading detection algorithms.

The system intelligently processes PDF documents to identify meaningful headings, filter out noise, and generate clean JSON outputs with proper hierarchical structure—all while maintaining high accuracy across different document types and languages.

## Key Features

- **Multi-Language Support** - Handles Latin, Devanagari (Hindi), and CJK (Chinese/Japanese/Korean) scripts
- **Intelligent Heading Detection** - Advanced scoring algorithms to identify true headings vs. noise
- **Hierarchical Structure** - Automatic H1, H2, H3 level assignment based on content analysis
- **TOC Processing** - Extracts existing table of contents when available
- **Visual Title Extraction** - Identifies document titles from layout and typography
- **Robust Filtering** - Eliminates false positives like dates, page numbers, and URLs
- **Lightweight & Fast** - Optimized Docker container with multi-stage build
- **JSON Output** - Structured, clean output format for further processing


## Technologies Used

- **PyMuPDF (fitz) 1.23.26** - Advanced PDF processing and text extraction
- **Python 3.10** - Core programming language with optimization flags
- **Docker Multi-stage Build** - Optimized containerization for production deployment
- **Regular Expressions** - Pattern matching for heading detection and filtering
- **Unicode Handling** - Multi-script text processing and normalization
- **JSON Processing** - Structured output generation

## Technical Approach

### 1. **Script Detection Algorithm**
```def detect_script(text): 
# Identifies document language: DEVANAGARI, CJK, or LATIN
# Enables language-specific processing optimizations
```


### 2. **Intelligent Heading Filtering**
- **Length Constraints**: 3-120 characters
- **Pattern Exclusion**: Dates, URLs, page numbers, percentages
- **Content Validation**: Must contain meaningful alphabetic characters
- **Noise Reduction**: Filters out common false positives

### 3. **Advanced Scoring System**
``` def score_span(span, body_size, script): 
# Multi-factor scoring based on:
# - Font size differential from body text
# - Font weight (bold) and styling
# - Position on page (Y-coordinate)
# - Text characteristics and patterns
# - Language-specific adjustments
```

### 4. **Hierarchical Level Assignment**
- **Pattern-based**: Numbered sections (1.1.1, 1.2, etc.)
- **Score-based**: High scores → H1, Medium → H2, Lower → H3
- **Position-aware**: Page order and vertical positioning

## How to Use

### Option 1: Docker Deployment (Recommended)

1. **Build the Docker image:**
``` docker build -t challenge1a . ```


2. **Run PDF processing:**
``` docker run --rm
-v "${PWD}/pdfs:/app/input"
-v "${PWD}/sample_dataset/outputs:/app/output"
challenge1a
```


3. **For Windows PowerShell:**
``` $inputDir = (Get-Location).Path + "\pdfs"
$outputDir = (Get-Location).Path + "\sample_dataset\outputs"
docker run --rm -v "${inputDir}:/app/input" -v "${outputDir}:/app/output" challenge1a
```


### Option 2: Local Python Execution

1. **Install dependencies:**
``` pip install PyMuPDF==1.23.26 ```


2. **Run the script:**
``` python process_pdfs.py ```


## Setup Instructions

### Step 1: Prepare Your Environment
Ensure Docker is installed on your system or Python 3.10+ for local execution.

### Step 2: Organize Your Files
Place all PDF documents to be processed in the `pdfs/` directory.

### Step 3: Execute Processing
Run the Docker command or Python script to process all PDFs and generate JSON outlines.

### Step 4: Review Outputs
Check the `sample_dataset/outputs/` directory for generated JSON files with extracted outlines.

## Output Format

Each processed PDF generates a JSON file with the following structure:
```
{
"title": "Document Title",
"outline": [
{
"level": "H1",
"text": "Main Heading",
"page": 0
},
{
"level": "H2",
"text": "Sub Heading",
"page": 1
},
{
"level": "H3",
"text": "Sub-sub Heading",
"page": 2
}
]
}

```


## Performance Optimizations

- **Multi-stage Docker Build**: Reduces final image size by excluding build dependencies
- **Python Optimizations**: Uses `-O` flag for bytecode optimization
- **Memory Efficient**: Processes documents individually to minimize memory footprint
- **Smart Filtering**: Early elimination of non-heading content to improve processing speed
- **Caching**: Efficient font size analysis for body text detection

## Error Handling

The system includes comprehensive error handling:
- **File Access Errors**: Graceful handling of corrupted or inaccessible PDFs
- **Memory Issues**: Proper document cleanup to prevent memory leaks
- **Processing Failures**: Returns structured error responses instead of crashes
- **Unicode Handling**: Robust multi-language text processing

## Supported Document Types

- **Technical Documents**: Research papers, manuals, specifications
- **Multi-language PDFs**: English, Hindi (Devanagari), Chinese, Japanese
- **Structured Documents**: Reports with clear heading hierarchies
- **Mixed Content**: Documents with tables, figures, and complex layouts

## Docker Container Details

- **Base Image**: `python:3.10-slim` for minimal footprint
- **Multi-stage Build**: Optimized for production deployment
- **Volume Mounts**: `/app/input` for PDFs, `/app/output` for results
- **Environment**: Optimized Python runtime with unbuffered output
- **Size**: Lightweight container optimized for PyMuPDF dependencies

## Troubleshooting

### Common Issues:

1. **Permission Errors**: Ensure Docker has access to mount directories
2. **Memory Issues**: For large PDFs, increase Docker memory allocation
3. **Font Issues**: Some PDFs may have embedded fonts that affect detection
4. **Language Detection**: Non-standard encodings may affect script detection

### Performance Tips:

- Process PDFs in batches for better throughput
- Use SSD storage for faster I/O operations
- Ensure adequate memory for large document processing

---

*This solution demonstrates advanced PDF processing capabilities with robust heading extraction, multi-language support, and production-ready containerization for the Adobe Hackathon 2025 Challenge 1A.*
