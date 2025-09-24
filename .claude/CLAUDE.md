# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

# Job Application Automation System - Memory & Instructions

## System Overview
An advanced job application automation system built entirely with Claude Code's multi-agent architecture. Uses native Anthropic features to analyze profiles and generate tailored application materials - no external scripts or dependencies required.

## Available Subagents

### Data Gathering Specialists
1. **github-specialist** - Analyzes GitHub repositories, extracts technical skills, and suggests profile enhancements
2. **linkedin-specialist** - Extracts LinkedIn profile data, analyzes professional brand, and provides optimization suggestions
3. **resume-analyst** - Parses Word/PDF resumes in multiple languages, extracts structured data, and provides ATS optimization

### Analysis & Optimization
4. **job-offer-specialist** - Analyzes job postings, extracts requirements, calculates fit scores (0-100), and provides application strategy
5. **profile-optimizer** - Consolidates all candidate data and provides comprehensive enhancement strategies

## Quick Start Commands

### Essential Commands
- `/analyze-candidate [github-url] [linkedin-url]` - Complete candidate profile analysis
- `/analyze-job [job-url]` - Analyze job posting and calculate fit score
- `/create-application [job-url]` - Generate tailored resume and cover letter
- `/full-pipeline [job-url]` - Execute complete application pipeline

### Workflow
1. **Initial Setup**: Place screenshots in `candidate-data/linkedin/screenshots/` and resumes in `candidate-data/resumes/resumes-files/`
2. **Profile Analysis**: Run `/analyze-candidate` (auto-detects files)
3. **Job Application**: Run `/full-pipeline [job-url]` for each job
4. **Review**: Check fit score and application materials in `job-offers/[company]/`

## Data Structure

```
Jobs/
├── candidate-data/         # Your profile data
│   ├── profile.json       # Master consolidated profile
│   ├── github/           # GitHub analysis
│   ├── linkedin/         # LinkedIn analysis
│   └── resumes/          # Resume files and analysis
├── job-offers/           # Job applications
│   └── [company]/        # Per-company folders
│       ├── job-analysis.json
│       ├── resume-tailored.docx
│       ├── cover-letter.docx
│       └── fit-score-report.json
└── logs/                 # System logs
```

## Fit Score Interpretation

The system calculates a realistic fit score (0-100) based on:
- **Hard Skills Match** (35%): Technical requirements alignment
- **Experience** (20%): Years and relevance
- **Education/Certs** (10%): Qualifications match
- **Soft Skills** (15%): Cultural and interpersonal fit
- **Logistics** (10%): Location, remote, availability
- **Compensation** (10%): Salary alignment

### Score Guidelines
- **90-100**: Excellent fit - Apply immediately
- **75-89**: Strong fit - High priority application
- **60-74**: Moderate fit - Apply with positioning
- **45-59**: Stretch role - Apply for growth
- **<45**: Poor fit - Generally skip

## Best Practices

### Profile Optimization
1. Keep your master profile updated weekly
2. Maintain consistency across GitHub, LinkedIn, and resumes
3. Quantify achievements with metrics
4. Use industry keywords naturally
5. Update skills based on market trends

### Application Strategy
1. Apply to 80+ fit scores within 48 hours
2. Customize materials for 70+ scores
3. Focus on 3-5 applications per day max
4. Follow up after 1 week
5. Track all applications in the system

### Quality Checks
- **ATS Score**: Must be 80+ for submission
- **Keyword Match**: Target 70%+ overlap
- **Grammar**: Zero errors required
- **Length**: 1-2 pages for resume
- **Formatting**: Clean, professional, consistent

## Claude Code Features

### Native Capabilities Used
- **Multi-Agent Orchestration**: Task tool for parallel execution
- **File Operations**: Direct Read/Write/Edit tools
- **Web Analysis**: WebFetch and WebSearch tools
- **Bash Integration**: Command execution without external scripts

### Parallel Processing
- All 5 subagents execute simultaneously
- 3x faster than sequential processing
- Smart data reuse between analyses
- No external orchestration required

## Troubleshooting

### Common Issues
1. **No screenshots found**: Add LinkedIn screenshots to `candidate-data/linkedin/screenshots/`
2. **No resumes found**: Add files to `candidate-data/resumes/resumes-files/`
3. **Low Fit Scores**: Update profile with missing skills
4. **Slow processing**: Ensure agents run in parallel (automatic)

### Data Locations
- Profile data in `candidate-data/`
- Job applications in `job-offers/[company]/`
- Agent definitions in `.claude/agents/`
- Commands in `.claude/commands/`

## Privacy & Security

### Protected Data
- Never commit `.env` files
- Exclude `candidate-data/private/`
- Sanitize API keys and tokens
- Use secure storage for passwords

### Data Handling
- All data stored locally
- No external API calls without permission
- Cached data expires after 24 hours
- Manual cleanup available

## Performance Tips

1. **Batch Processing**: Analyze multiple jobs together
2. **Cache Usage**: Reuse analyzed profiles for 24h
3. **Parallel Execution**: Subagents run concurrently
4. **Selective Analysis**: Skip unchanged data sources
5. **Quick Mode**: Use fit score for filtering

## Architecture

Built with pure Claude Code features:
- No Python scripts required
- No external dependencies
- Native parallel execution
- Direct tool invocations

## Version
System Version: 2.0.0
Last Updated: 2025
Powered by: Claude Code (Anthropic)
Architecture: Pure multi-agent system