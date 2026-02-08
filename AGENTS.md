# AGENTS.md - Azure Training Extract

## Project Overview

A Python tool that extracts learning content from Microsoft Learn courses and converts them to organized HTML and PDF files for offline reading. The tool uses the Microsoft Learn Catalog API to discover course structure and Playwright for PDF generation.

## Project Structure

```
.
├── main.py              # Main script with all classes and logic
├── requirements.txt     # Python dependencies
├── README.md           # User-facing documentation
├── AGENTS.md           # This file - agent guidance
├── output/             # Generated output directory (created at runtime)
└── .venv/              # Virtual environment
```

## Architecture

The project uses an object-oriented design with clear separation of concerns:

| Class | File | Responsibility |
|-------|------|----------------|
| `HttpClient` | `main.py` | HTTP requests with consistent configuration |
| `CatalogService` | `main.py` | Interacts with Microsoft Learn Catalog API |
| `ContentService` | `main.py` | Fetches and processes web page content |
| `HtmlGenerator` | `main.py` | Generates combined HTML documents |
| `PdfGenerator` | `main.py` | Converts HTML to PDF using Playwright |
| `CourseProcessor` | `main.py` | Orchestrates the entire extraction workflow |
| `PageContent` | `main.py` | Data class for page content |

## Key Constants

Located at the top of `main.py`:

```python
DEFAULT_COURSE_URL = "https://learn.microsoft.com/en-us/training/courses/ai-102t00"
CATALOG_API_URL = "https://learn.microsoft.com/api/catalog/"
OUTPUT_BASE_DIR = "output"
REQUEST_TIMEOUT = 30
CATALOG_TIMEOUT = 60
```

## Dependencies

From `requirements.txt`:
- `requests` - HTTP library
- `beautifulsoup4` - HTML parsing
- `playwright` - Browser automation for PDF generation

## Coding Guidelines

### Style Conventions

1. **Imports**: Group in order - stdlib, third-party, local
2. **Docstrings**: Use triple quotes with brief description
3. **Type Hints**: Use `Optional[Type]` for nullable params
4. **Naming**: 
   - Classes: `PascalCase`
   - Functions/variables: `snake_case`
   - Constants: `UPPER_SNAKE_CASE`
5. **Private methods**: Prefix with underscore `_method_name`

### Adding New Features

1. **New Service**: Create a new class following existing patterns
2. **Configuration**: Add constants at the top of `main.py`
3. **Output Handling**: Use `OUTPUT_BASE_DIR` as base directory
4. **Error Handling**: Print errors and return safe defaults

## Common Tasks

### Adding a New Output Format

1. Create a new generator class (e.g., `MarkdownGenerator`)
2. Add generation method in `CourseProcessor`
3. Call from `_process_module()` method

### Modifying HTML Output

1. Update `HTML_STYLES` constant for CSS changes
2. Modify `HtmlGenerator._build_section()` for section structure
3. Modify `HtmlGenerator._build_document()` for document structure

### Changing Content Extraction

1. Modify `ContentService._extract_content()` for main content
2. Update `ContentService._clean_navigation_elements()` to remove elements
3. Adjust `ContentService._extract_title()` for title extraction

## Testing

No formal test suite exists. Test manually by running:

```bash
python main.py
```

Then press Enter to use the default course URL and verify output in `output/`.

## Important Notes

1. **Playwright Requirement**: `playwright install chromium` must be run after pip install
2. **Network Dependency**: Requires internet access to Microsoft Learn
3. **Rate Limiting**: No rate limiting is implemented - be mindful when testing
4. **Output Directory**: Created automatically if it doesn't exist
5. **File Sanitization**: Filenames are sanitized to remove invalid characters

## Troubleshooting

| Issue | Solution |
|-------|----------|
| PDF generation fails | Ensure Playwright browsers are installed |
| Empty output | Check network connection and URL validity |
| Missing content | Microsoft Learn page structure may have changed |
| Timeout errors | Increase `REQUEST_TIMEOUT` or `CATALOG_TIMEOUT` |
