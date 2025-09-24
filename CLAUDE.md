# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## System Architecture

This is a job application automation system built with Claude's agent architecture. It analyzes candidate profiles from screenshots and files, then generates optimized application materials for each job opportunity.

### Core Components

**Subagents** (`.claude/agents/`)
- `linkedin-specialist`: Analyzes LinkedIn profile screenshots from `candidate-data/linkedin/screenshots/`
- `resume-analyst`: Auto-processes all files in `candidate-data/resumes/resumes-files/`
- `github-specialist`: Analyzes GitHub repositories when URL provided
- `job-offer-specialist`: Analyzes job postings and calculates fit scores (0-100)
- `profile-optimizer`: Consolidates all data sources into master profile

**Commands** (`.claude/commands/`)
- `/analyze-candidate`: Parallel analysis of screenshots + resumes + GitHub
- `/full-pipeline [job-url]`: Complete application generation pipeline
- `/test-system`: Tests all components with parallel execution

### Data Flow

1. **Smart Cache Check**: Reuses analysis < 1 week old (60% faster)
2. **Input Sources** (auto-detected):
   - LinkedIn: `candidate-data/linkedin/screenshots/*.png`
   - Resumes: `candidate-data/resumes/resumes-files/*.(pdf|docx)`
   - GitHub: URL provided as argument
3. **Processing**: Parallel execution of all agents (3x faster)
4. **Outputs**:
   - Analysis cached in `candidate-data/*/`
   - Applications in `job-offers/[company]/`

## Critical: Parallel Execution

**MUST** send all Task invocations in ONE message for parallel execution:

```xml
<Task>linkedin-specialist</Task>
<Task>resume-analyst</Task>
<Task>job-offer-specialist</Task>
<!-- All in ONE message = parallel (3x faster) -->
```

Never send Tasks in separate messages (sequential = slow).

## Data Accuracy Rules

1. **NO HALLUCINATIONS**: Only extract visible text from screenshots
2. **EXACT EXTRACTION**: Copy names/dates exactly as shown
3. **VERIFY DATA**: Cross-check across sources before writing

## Configuration

The system uses pure Claude Code features without external scripts:
- Native multi-agent orchestration via Task tool
- Built-in parallel execution capabilities
- Direct file operations (Read/Write/Edit)
- No external Python dependencies required

## Common Workflows

### Analyze Candidate Profile
```bash
# Auto-detects screenshots and resumes
/analyze-candidate

# With GitHub URL
/analyze-candidate https://github.com/username
```

### Apply to Job
```bash
# First job: Full analysis (~8 min)
/full-pipeline https://job-url-1.com

# Next jobs: Uses cached profile (~3 min each)
/full-pipeline https://job-url-2.com
/full-pipeline https://job-url-3.com

# Generates in job-offers/[company]/
# Cache auto-refreshes after 1 week
```

### Test System
```bash
# Tests parallel execution
/test-system pipeline
```

## File Locations

**Input Data**:
- LinkedIn screenshots: `candidate-data/linkedin/screenshots/`
- Resume files: `candidate-data/resumes/resumes-files/`

**Analysis Results**:
- LinkedIn: `candidate-data/linkedin/profile-analysis.json`
- Resumes: `candidate-data/resumes/resume-analysis.json`
- GitHub: `candidate-data/github/analysis.json`
- Master: `candidate-data/analysis/complete-profile.json`

**Job Applications**:
- `job-offers/[company]/job-analysis.json`
- `job-offers/[company]/resume-tailored.docx`
- `job-offers/[company]/cover-letter.docx`

## Fit Score Calculation

- **90-100**: Apply immediately (excellent fit)
- **75-89**: Priority application (strong fit)
- **60-74**: Apply with customization (moderate)
- **45-59**: Growth opportunity (stretch)
- **<45**: Skip (poor fit)

Components:
- Hard Skills (35%), Experience (20%), Education (10%)
- Soft Skills (15%), Logistics (10%), Compensation (10%)

## Performance Benchmarks

- **First job application**: ~8 minutes (full profile + job analysis)
- **Subsequent applications**: ~3 minutes (smart caching)
- **Cache duration**: 1 week for profile data
- **Parallel execution**: 3x faster than sequential
- **Total speedup**: 60% faster with caching + parallel

## Current User Profile

**Yassine Senhaji**:
- AI Architect with LLMs, Python, RAG expertise
- 7 years at Attijariwafa Bank Europe
- Entrepreneurial ventures (€450K+ revenue)
- GitHub: Loryo80 (multi-agent systems)
- Score: 88/100 (Exceptional candidate)