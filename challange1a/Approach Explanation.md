# Approach Explanation – Adobe Hackathon Round 1A

## Objective

Our solution aims to intelligently extract structured outlines (titles and hierarchical headings H1/H2/H3) from PDF documents with **multi-language support** and **advanced heading detection algorithms**, fully aligned with Adobe's "Connecting the Dots" theme. The system operates entirely **offline**, requires **no ML models**, and processes PDFs in **under 10 seconds** per document.

---

## Workflow Overview

### 1. Multi-Language Script Detection
The system begins by analyzing text content to identify the document's primary script:
- **DEVANAGARI** (Hindi/Sanskrit scripts)
- **CJK** (Chinese/Japanese/Korean characters)  
- **LATIN** (English and European languages)
> This enables language-specific processing optimizations for heading detection.

---

### 2. Intelligent Body Text Analysis
To accurately identify headings, we first determine the document's **body text font size** by:
- Analyzing all text spans across pages
- Filtering out headers, footers, and obvious headings
- Calculating the most frequent font size for regular paragraph text
- Using this as a baseline for heading size comparison

> This approach ensures accurate heading detection regardless of document formatting variations.

---

### 3. Advanced Heading Scoring Algorithm
Each text span is evaluated using a **multi-factor scoring system**:

**Size-based Scoring:**
- Font size differential from body text (higher = more likely heading)
- Minimum threshold ensures only significantly larger text qualifies

**Typography Scoring:**
- Bold font weight detection
- Font family analysis
- Text styling flags

**Position-based Scoring:**
- Y-coordinate analysis (top of page = higher score)
- X-coordinate for left-alignment detection
- Page boundary considerations

**Content Validation:**
- Word count optimization (2-8 words ideal for headings)
- Pattern recognition for numbered sections (1.1.1, 1.2, etc.)
- Title case and uppercase detection

---

### 4. Robust Noise Filtering
Our system eliminates false positives by filtering out:
- **Dates and timestamps** (various formats)
- **Page references** ("page 1 of 10")
- **URLs and email addresses**
- **Version numbers** and document metadata
- **Figure/table captions**
- **Parenthetical content**
- **Currency and percentage values**

---

### 5. Hierarchical Level Assignment
Headings are assigned levels through:

**Pattern-based Assignment:**
- `1.1.1` format → H3
- `1.1` format → H2  
- `1.` format → H1

**Score-based Assignment:**
- High scores (12+) → H1
- Medium scores (8-11) → H2
- Lower scores (7+) → H3

**Position-aware Sorting:**
- Primary sort by page number
- Secondary sort by Y-coordinate position

---

## Output Format

The final output JSON follows Adobe's specification exactly:
```
{
"title": "Document Title",
"outline": [
{ "level": "H1", "text": "Introduction", "page": 0 },
{ "level": "H2", "text": "Methodology", "page": 1 }
]
}
```

---

## ✅ Constraints Respected

| Constraint            | Compliant |
|-----------------------|-----------|
| Offline Execution     | ✅ Yes    |
| No ML Models Required | ✅ Yes (0MB) |
| CPU-only Processing   | ✅ Yes    |
| Execution < 10 sec    | ✅ Yes (~2-5 sec) |
| AMD64 Architecture    | ✅ Multi-stage Docker |
| Output Format         | ✅ Fully Aligned |

---

## Technologies Used

- **PyMuPDF (fitz) 1.23.26** for advanced PDF parsing and text extraction
- **Python 3.10** with optimization flags for performance
- **Regular Expressions** for pattern matching and filtering
- **Unicode Processing** for multi-language text handling
- **Multi-stage Docker Build** for optimized container size
- **JSON Processing** for structured output generation

---

## Why This Works

Instead of relying solely on **font size heuristics** (which can be unreliable), our system uses a **comprehensive scoring algorithm** that considers:

1. **Typography** - Font size, weight, and styling
2. **Layout** - Spatial positioning and page structure  
3. **Content** - Text patterns and linguistic characteristics
4. **Context** - Document flow and hierarchical relationships

The result: **Highly accurate heading detection** across diverse document types and languages, with robust filtering to eliminate noise and false positives.

---

## Key Innovations

### 1. **Multi-Language Intelligence**
- Native support for Latin, Devanagari, and CJK scripts
- Language-specific font weight detection algorithms
- Unicode normalization for consistent processing

### 2. **Dynamic Body Text Detection**
- Adaptive baseline calculation per document
- Eliminates assumptions about "standard" font sizes
- Works across different document templates and styles

### 3. **Advanced Filtering Pipeline**
- Comprehensive regex patterns for noise elimination
- Length and content validation
- Duplicate detection and removal

### 4. **Production-Ready Architecture**
- Multi-stage Docker build for minimal image size
- Memory-efficient processing with proper cleanup
- Error handling with graceful degradation

---

## Performance Optimizations

- **Early Filtering**: Eliminates non-heading content before expensive processing
- **Efficient Font Analysis**: Smart sampling of body text for baseline calculation
- **Memory Management**: Proper document cleanup to prevent memory leaks
- **Optimized Python**: Uses `-O` flag for bytecode optimization
- **Minimal Dependencies**: Only PyMuPDF required, no heavy ML frameworks

---

## Future Improvements

- Enhanced TOC extraction for documents with embedded navigation
- Machine learning integration for even more accurate heading classification
- Support for additional scripts (Arabic, Thai, etc.)
- Advanced layout analysis for complex multi-column documents

---

## Thank You

We're excited to help reimagine the future of intelligent document processing with Adobe, delivering **blazing-fast, accurate, and multilingual** PDF structure extraction that forms the foundation for next-generation document experiences.
