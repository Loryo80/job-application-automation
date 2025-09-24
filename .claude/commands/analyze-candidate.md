---
allowed-tools: Task, Read, Write, Bash, Glob
argument-hint: (no arguments needed - uses screenshots and resume files)
description: Complete candidate analysis from LinkedIn screenshots and resume files - runs all agents in parallel
model: opus
---

# Comprehensive Candidate Analysis Pipeline

You are initiating a complete candidate profile analysis using screenshots and resume files.

## Data Sources (No URLs needed!)
- **LinkedIn Screenshots**: `candidate-data/linkedin/screenshots/`
- **Resume Files**: `candidate-data/resumes/resumes-files/`
- **GitHub**: (optional - if URL provided)

## IMPORTANT: Parallel Execution
Execute ALL agents in parallel for maximum performance using a single Task tool message with multiple invocations.

## Execution Plan

### Parallel Phase 1: Data Collection (Run ALL simultaneously)

Use Task tool with PARALLEL execution for:

1. **linkedin-specialist** agent:
   - Analyze ALL screenshots in `candidate-data/linkedin/screenshots/`
   - Extract profile data from images
   - No URL needed - works with screenshots

2. **resume-analyst** agent:
   - Process ALL files in `candidate-data/resumes/resumes-files/`
   - Extract and compare all resume versions
   - No file paths needed - auto-scans folder

3. **github-specialist** agent (if GitHub URL provided):
   - Analyze repositories
   - Extract technical skills
   - Optional - only if URL in arguments

### Phase 2: Profile Optimization

AFTER all parallel tasks complete, run profile-optimizer:

```
<Task>
  <description>Profile optimization</description>
  <prompt>Consolidate all analysis results from LinkedIn, resumes, and GitHub (if available) to create unified profile</prompt>
  <subagent_type>profile-optimizer</subagent_type>
</Task>
```

This consolidates findings from all sources and creates optimization roadmap.

## Execution Instructions

1. **Check Data Availability**:
   ```bash
   ls candidate-data/linkedin/screenshots/
   ls candidate-data/resumes/resumes-files/
   ```

2. **CRITICAL: Execute Parallel Analysis**

   You MUST use the Task tool with multiple invocations in a SINGLE message like this:

   ```
   <Task>
     <description>LinkedIn analysis</description>
     <prompt>Analyze LinkedIn screenshots in candidate-data/linkedin/screenshots/</prompt>
     <subagent_type>linkedin-specialist</subagent_type>
   </Task>
   <Task>
     <description>Resume analysis</description>
     <prompt>Process all resume files in candidate-data/resumes/resumes-files/</prompt>
     <subagent_type>resume-analyst</subagent_type>
   </Task>
   <Task>
     <description>GitHub analysis</description>
     <prompt>Analyze GitHub profile [URL if provided]</prompt>
     <subagent_type>github-specialist</subagent_type>
   </Task>
   ```

   ⚠️ IMPORTANT: All Task invocations must be in ONE message for parallel execution!

3. **Consolidate Results**:
   - Wait for all agents to complete
   - Run profile-optimizer with all results

## Output Structure

```
candidate-data/
├── linkedin/
│   ├── screenshots/          # Input screenshots
│   └── profile-analysis.json # LinkedIn analysis
├── resumes/
│   ├── resumes-files/        # Input resume files
│   └── resume-analysis.json  # Resume analysis
├── github/
│   └── analysis.json         # GitHub analysis (if applicable)
└── analysis/
    └── complete-profile.json # Consolidated analysis
```

## Success Metrics
- All screenshots processed ✓
- All resume files analyzed ✓
- Data cross-referenced ✓
- No hallucinations ✓
- Parallel execution completed ✓

## Error Handling
- If no screenshots: Report and request LinkedIn screenshots
- If no resumes: Report and request resume files
- If data missing: Proceed with available data, note gaps

## Final Report includes:
1. Unified professional profile
2. Skills from all sources
3. Experience timeline (reconciled)
4. Optimization priorities
5. Action plan with timelines
6. ATS optimization suggestions
7. LinkedIn enhancement recommendations