# Programmatic SEO

An end-to-end automated SEO toolkit for keyword research, SERP analysis, content creation, and AI-friendly optimization to boost search and AI rankings.

## Overview

This toolkit provides automated SEO workflows powered by AI. It includes skills for generating high-quality, SEO-optimized content that performs well in both traditional search engines and generative AI systems.

## Features

- **Keyword Research**: Automated keyword discovery and expansion
- **SERP Analysis**: Deep analysis of search engine results pages
- **Content Generation**: AI-powered article writing with multiple framework options
- **SEO Optimization**: Metadata, schema markup, and GEO optimization
- **AEO/GEO Support**: Optimized for Answer Engine Optimization and Generative Engine Optimization

## Skills

### Programmatic SEO Writer

A fully integrated, single-pipeline SEO + GEO content production skill.

**Triggers:**
- `write article: [keyword]`
- `generate blog post`
- `create SEO content`
- `show available keywords`
- `force framework [A/B/C/D]: [keyword]`

**Pipeline:**
1. Keyword Backlog Check
2. Keyword Research & Expansion
3. SERP Deep Analysis
4. Article Writing
5. Four-Block Output (Metadata, Article, FAQ+Schema, GEO Version)

See [skills/programmatic-seo-writer.md](skills/programmatic-seo-writer.md) for detailed documentation.

## Usage

This toolkit is designed to work with AI agents that support skill-based workflows. Import the desired skill to enable automated SEO content generation.

## Project Structure

```
programmatic-seo/
├── README.md
├── LICENSE
└── skills/
    └── programmatic-seo-writer.md
```

## License

MIT License - see LICENSE file for details.

## Contributing

Contributions are welcome! Please feel free to submit issues and pull requests.
