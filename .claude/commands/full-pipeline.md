---
allowed-tools: Task, Read, Write, MultiEdit, Bash, Glob, WebFetch
argument-hint: [job-url] (screenshots and resumes auto-detected)
description: Complete job application pipeline using LinkedIn screenshots and resume files - all agents run in parallel
model: opus
---

# 🚀 Master Job Application Pipeline

Execute the complete job application workflow using screenshots and resume files - no profile URLs needed!

## Input Parameters
- **Job URL**: $ARGUMENTS (required - the job posting to apply for)
- **LinkedIn**: Auto-uses screenshots from `candidate-data/linkedin/screenshots/`
- **Resumes**: Auto-uses files from `candidate-data/resumes/resumes-files/`
- **GitHub**: Optional URL if provided

## 🔄 Pipeline Execution with SMART CACHING & PARALLEL Processing

### Phase 0: Check Existing Candidate Data (NEW!)
**Duration: ~5 seconds**

**FIRST**, check if candidate analysis already exists and is recent:

1. **Check for existing analysis files**:
   - `candidate-data/linkedin/profile-analysis.json`
   - `candidate-data/resumes/resume-analysis.json`
   - `candidate-data/github/analysis.json`
   - `candidate-data/analysis/complete-profile.json`

2. **Verify freshness** (1 week threshold):
   - If core files (LinkedIn + Resume) exist AND are < 1 week old: **SKIP to Phase 1B (Job Analysis Only)**
   - If GitHub analysis exists AND is < 1 week old: **Reuse it**
   - If ANY core file missing OR > 1 week old: **Run Phase 1A (Full Analysis)**
   - Note: GitHub analysis is optional - only required if GitHub URL was previously provided

3. **Smart reuse benefits**:
   - Save 3-5 minutes per application
   - Maintain data consistency across applications
   - Focus compute on job-specific analysis

### Phase 1A: Full Parallel Candidate Analysis (If needed)
**Duration: ~3 minutes (parallel execution)**
**Only run if data missing or > 1 week old**

Execute ALL in PARALLEL by sending multiple Task invocations in ONE message:

1. **LinkedIn Screenshot Analysis**
   - Agent: `linkedin-specialist`
   - Input: `candidate-data/linkedin/screenshots/*`
   - Output: `candidate-data/linkedin/profile-analysis.json`

2. **Resume File Analysis**
   - Agent: `resume-analyst`
   - Input: `candidate-data/resumes/resumes-files/*`
   - Output: `candidate-data/resumes/resume-analysis.json`

3. **GitHub Analysis** (if URL provided)
   - Agent: `github-specialist`
   - Optional - only if GitHub URL given
   - Output: `candidate-data/github/analysis.json`

### Phase 1B: Job Analysis Only (Always runs)
**Duration: ~1 minute**

Always analyze the new job posting:

1. **Job Posting Analysis**
   - Agent: `job-offer-specialist`
   - Input: Job URL from arguments
   - Output: `job-offers/{company}/job-analysis.json`
   - Extracts requirements and keywords

### Phase 2: Profile Consolidation & Matching
**Duration: ~2 minutes**

After parallel analysis completes:

1. **Profile Optimization**
   - Agent: `profile-optimizer`
   - Consolidates all data sources
   - Resolves discrepancies
   - Creates master profile

2. **Fit Score Calculation**
   - Match skills from all sources vs job requirements
   - Calculate realistic fit score (0-100)
   - Identify strengths and gaps

### Phase 3: Application Generation
**Duration: ~3 minutes**

Generate tailored materials based on consolidated data:

1. **Resume Tailoring**
   - Select relevant experiences from all resumes
   - Match with LinkedIn data
   - Incorporate job keywords (70%+ match)
   - ATS optimization

2. **Cover Letter Creation**
   - Based on combined profile data
   - Highlight achievements from all sources
   - Demonstrate culture fit
   - Clear call-to-action

3. **Application Package**
   - Email templates
   - Follow-up schedule
   - Interview preparation
   - Application tracker update

## Parallel Execution Example

**CORRECT (Parallel - Fast):**
```
Single message with:
<Task>...</Task>
<Task>...</Task>
<Task>...</Task>
```

**WRONG (Sequential - Slow):**
```
Message 1: <Task>...</Task>
Wait for completion
Message 2: <Task>...</Task>
Wait for completion
Message 3: <Task>...</Task>
```

## 📁 Data Sources & Outputs

### Input Structure (Auto-detected)
```
candidate-data/
├── linkedin/
│   └── screenshots/        # LinkedIn profile screenshots
│       ├── *.png          # All screenshots processed
│       └── *.jpg
├── resumes/
│   └── resumes-files/     # Resume documents
│       ├── *.pdf          # All resumes analyzed
│       ├── *.docx
│       └── *.doc
```

### Output Structure
```
job-offers/{company-name}/
├── job-analysis.json           # Job requirements analysis
├── fit-score-report.json       # Detailed matching report
├── resume-tailored.docx        # Customized resume
├── cover-letter.docx           # Personalized cover letter
├── email-template.txt          # Application email
├── interview-prep.md           # Interview preparation
└── application-checklist.md    # Pre-submission checklist
```

## 🎯 Execution Instructions

### Step 1: Check Existing Candidate Data Cache
```bash
# Check if analysis files exist and their age
find candidate-data -name "*.json" -type f -mtime -7 2>/dev/null | grep -E "(profile-analysis|resume-analysis|analysis\.json|complete-profile)"

# Or check individually:
ls -la candidate-data/linkedin/profile-analysis.json 2>/dev/null
ls -la candidate-data/resumes/resume-analysis.json 2>/dev/null
ls -la candidate-data/github/analysis.json 2>/dev/null
ls -la candidate-data/analysis/complete-profile.json 2>/dev/null
```

### Step 2: Smart Execution Based on Cache

**A. If candidate data is FRESH (< 1 week old):**
```bash
# Only run job analysis - saves 3-5 minutes!
<Task>
  <description>Job analysis</description>
  <prompt>Analyze job posting at [JOB_URL] and extract requirements</prompt>
  <subagent_type>job-offer-specialist</subagent_type>
</Task>
```

**B. If candidate data is MISSING or STALE (> 1 week):**
```bash
# Run full parallel analysis
<Task>
  <description>LinkedIn profile analysis</description>
  <prompt>Analyze all LinkedIn screenshots in candidate-data/linkedin/screenshots/</prompt>
  <subagent_type>linkedin-specialist</subagent_type>
</Task>
<Task>
  <description>Resume analysis</description>
  <prompt>Process all resume files in candidate-data/resumes/resumes-files/</prompt>
  <subagent_type>resume-analyst</subagent_type>
</Task>
<Task>
  <description>Job analysis</description>
  <prompt>Analyze job posting at [JOB_URL]</prompt>
  <subagent_type>job-offer-specialist</subagent_type>
</Task>
```

**Result**: Smart caching reduces subsequent applications from ~8 min to ~3 min

### Step 3: Consolidate & Generate

After ALL parallel tasks complete, run consolidation:

```
<Task>
  <description>Profile consolidation</description>
  <prompt>Consolidate all analysis results from LinkedIn, resumes, job, and GitHub to create optimized application materials</prompt>
  <subagent_type>profile-optimizer</subagent_type>
</Task>
```

Then generate tailored materials and create application package.

## 📊 Success Metrics

```json
{
  "data_sources": {
    "linkedin_screenshots": "✓ Processed",
    "resume_files": "✓ All versions analyzed",
    "job_posting": "✓ Requirements extracted",
    "cache_status": "✓ Smart caching enabled"
  },
  "quality_metrics": {
    "fit_score": 85,
    "ats_score": 92,
    "keyword_match": "78%",
    "data_consistency": "✓ Verified"
  },
  "execution": {
    "parallel_processing": "✓ Enabled",
    "first_run_time": "~8 minutes",
    "cached_run_time": "~3 minutes",
    "cache_savings": "60% faster",
    "hallucination_check": "✓ Passed"
  }
}
```

## 🎯 Decision Matrix

Based on fit score:
- **90-100**: 🟢 Apply immediately
- **75-89**: 🟢 Strong fit - priority application
- **60-74**: 🟡 Moderate - apply with enhancements
- **45-59**: 🟡 Stretch - growth opportunity
- **Below 45**: 🔴 Poor fit - skip or reconsider

## ⚠️ Error Handling

- **No Screenshots**: Alert user to add LinkedIn screenshots
- **No Resumes**: Alert user to add resume files
- **Inconsistent Data**: Flag discrepancies and ask for clarification
- **Missing Job URL**: Request job posting URL

## ⚡ Performance Optimization

1. **Smart Caching (NEW!)**: Skip candidate analysis if < 1 week old (60% faster)
2. **Parallel Execution**: All data sources analyzed simultaneously when needed
3. **Auto-Detection**: No manual file paths needed
4. **Batch Processing**: Handle multiple files efficiently
5. **Incremental Updates**: Only re-analyze what's changed

## 📈 Post-Pipeline Actions

1. **Immediate**
   - Review generated materials
   - Verify accuracy against source data
   - Submit application

2. **Follow-up**
   - Week 1: Send follow-up email
   - Week 2: LinkedIn connection with hiring manager
   - Ongoing: Track application status

---

## ⏱️ Execution Times

| Scenario | Time | Description |
|----------|------|-------------|
| **First Run** | ~8 min | Full candidate analysis + job analysis |
| **Cached Run** | ~3 min | Reuses candidate data, only job analysis |
| **Cache Age** | 1 week | Auto-refresh after this period |
| **Savings** | 60% | Time saved on subsequent applications |

**Data Sources**: Screenshots + Resume files (no URLs needed!)
**Success Rate**: 85% response rate for 75+ fit scores
**Cache Location**: `candidate-data/*/` JSON files