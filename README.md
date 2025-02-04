# Web to Markdown Converter

## Author: [Jon Pappas](https://github.com/mindfulent)

## Version: 1.1

A Python-based tool that transforms web content into clean, well-formatted Markdown documents. It uses gpt-4o-mini with its vision capabilties to first determine what are the most important parts of the page to focus on (i.e. no navigation, no ads, etc). Then it parses the HTML comparing it to the screenshot to draft a proposal for a markdown representation. Finally, it reviews the content and references some guidelines to ensure the final output meets markdown standards. This tool uses [docs.ell.so](https://docs.ell.so) to orchestrate the process and to permit inspection of the prompts, OpenAI for the LLM calls, Selenium for the screenshot capture and BeautifulSoup for the HTML parsing. Cursor and Claude 3.5 Sonnet were used to build this project.

## Why?

In an era of AI-powered development tools like [Cursor.com](https://cursor.sh), having web content in Markdown format enables collaboration with AI by providing structured, easily referenceable documentation. Markdown's widespread adoption in developer workflows, native rendering in GitHub, and support across various documentation platforms makes it a good candidate for preserving and sharing web content in both a human-friendly and AI-context-friendly way.

For intance, to build this tool I had to use Ell.so. But in order to understand Ell.so, which is very fresh and new, I had to create context files to feed to Claude within Cursor.com to know how to use it properly. This tool was used to create [docs/notes_ell_combined.md][ell-notes] which you can use in other Ell.so-powered projects.

[ell-notes]: https://github.com/mindfulent/WebToMd/blob/master/docs/notes_ell_combined.md

## Features

- Intelligent content extraction using visual analysis via gpt-4o-mini
- HTML parsing with BeautifulSoup
- Visual capture using Selenium
- Clean Markdown formatting with customizable rules
- Comprehensive error handling and logging
- Progress tracking for conversion stages
- (New in 1.1) Batch processing via YAML configuration
- (New in 1.1) Custom prefix support for organized output files

## Example Output

Check out [Markdown.md](archive/demo/Markdown.md) for an example of the converter's output when processing the Wikipedia article on Markdown (<https://en.wikipedia.org/wiki/Markdown>). This example demonstrates the tool's ability to:

- Preserve document structure and hierarchy
- Handle complex formatting including tables and code blocks
- Maintain proper link references
- Clean up unnecessary content while keeping essential information
- Format content according to markdown best practices

Another example: 

 - [archive/demo/Balloon_Fight.png][balloon-png]
 - [archive/demo/Balloon_Fight.md][balloon-md]

[balloon-png]: archive/demo/Balloon_Fight.png
[balloon-md]: archive/demo/Balloon_Fight.md

## Prerequisites

- Python 3.11 or higher
- Chrome/Chromium browser (for Selenium)
- OpenAI via Ell.so framework

## Installation

1. Clone the repository:

```bash
git clone https://github.com/mindfulent/WebToMd
cd WebToMd
```

2. Create and configure your environment file:

```bash
# Copy the sample environment file
# Update with your OpenAI API key
cp .env.sample .env
```

3. Set up Python environment and install dependencies:

```bash
# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # On Windows use: venv\Scripts\activate

# Install requirements
pip install -r requirements.txt
```

4. Start Ell Studio first so that you can inspect the output:

```bash
ell-studio --storage ./logdir
```

5. Run the converter:
```bash
# Run interactively
python -m src.convert

# Run with URL argument (using Fireworks.ai documentation as an example)
python -m src.convert --url https://docs.fireworks.ai/getting-started/introduction 

# Run batch processing from config file with a custom prefix (using ALL of Fireworks.ai documentation as an example, 97 pages)
cp archive/configs/fireworks.config.yml src/config.yml
python -m src.convert --config src/config.yml --prefix fireworks
```

6. To deactivate the virtual environment when you're done:

```bash
deactivate
```

## Project Structure

```text
WebToMd/
├── archive/
│  ├── configs/
│  │  └── fireworks.config.yml   # Batch processing config file example
│  └── demo/
│     ├── Balloon_Fight.png      # Screenshot example
│     ├── Balloon_Fight.md       # Markdown example   
│     ├── Markdown.png           # Screenshot example
│     └── Markdown.md            # Markdown example
├── docs/
│  ├── ell-context.md            # Context file for Ell.so
│  └── markdown-context.md       # Context file for Markdown standards
├── src/
│ ├── convert.py                 # Main conversion logic
│ ├── combine.py                 # Combine context files
│ └── config.yml                 # Batch processing config file example
├── output/                      # Output directory
│  └── screenshots/              # Screenshot output
├── README.md                    # README file
└── requirements.txt
```

## Configuration

The application can be configured through environment variables:

- `OPENAI_API_KEY`: OpenAI API key
- `CHROME_BINARY_PATH`: Path to Chrome/Chromium binary

## Processing Pipeline

1. **Visual Analysis Phase**
   - Screenshot capture
   - Visual importance analysis
   - Content area identification

2. **Content Extraction Phase**
   - Targeted HTML extraction
   - Content cleaning
   - Element correlation

3. **Markdown Conversion Phase**
   - Structured conversion
   - Format preservation
   - Quality validation

## Config File Format

The application supports batch processing using a YAML configuration file. Create a `config.yml` file with the following structure:

```yaml
urls:
  - https://example.com/page1
  - https://example.com/page2
  - https://example.com/page3
```

When using batch processing with a prefix, output files will be generated with sequential numbering:

- prefix-01-page1.md
- prefix-02-page2.md
- prefix-03-page3.md

This makes it easy to maintain order and organization when processing multiple related pages.
