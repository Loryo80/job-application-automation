---
allowed-tools: Task, Read, Write, Bash, Glob
argument-hint: [component: linkedin|resume|job|optimizer|pipeline|all]
description: Test system components using screenshots and resume files - parallel execution enabled
model: opus
---

# 🧪 System Testing Framework

Test the job application system using screenshots and resume files - no URLs needed!

## Test Component: $ARGUMENTS

## Data Sources for Testing
- **LinkedIn**: Screenshots in `candidate-data/linkedin/screenshots/`
- **Resumes**: Files in `candidate-data/resumes/resumes-files/`
- **Job URLs**: Sample job postings for testing

## Test Execution Plan

### 1. Pre-Test Setup
```bash
# Check available test data
echo "=== Checking LinkedIn Screenshots ==="
ls -la candidate-data/linkedin/screenshots/

echo "=== Checking Resume Files ==="
ls -la candidate-data/resumes/resumes-files/

# Create test output directories
mkdir -p tests/outputs
mkdir -p tests/logs

# Log test start
echo "Test started: $(date)" > tests/logs/test-$(date +%Y%m%d).log
```

### 2. Component-Specific Tests (PARALLEL EXECUTION)

#### Test: linkedin-specialist (Screenshot-based)
```
Input: candidate-data/linkedin/screenshots/*
Expected Output:
- Profile data extraction from images
- Experience parsing from screenshots
- Skills identification
- Optimization recommendations
```

Validation:
- Check if `candidate-data/linkedin/profile-analysis.json` exists
- Verify data extracted from screenshots
- No hallucinations (only data visible in images)
- Attijariwafa Bank correctly identified (not BforBank)

#### Test: resume-analyst (Auto-folder scan)
```
Input: candidate-data/resumes/resumes-files/*
Expected Output:
- All PDF/DOCX files processed
- Multi-version comparison
- ATS score calculation
- Consolidated profile data
```

Validation:
- All resume files in folder processed
- `candidate-data/resumes/resume-analysis.json` created
- Individual file analyses created
- No manual file paths needed

#### Test: job-offer-specialist
```
Sample Job URLs for Testing:
- https://jobs.lever.co/[company]/[job-id]
- https://careers.[company].com/job/[id]
- LinkedIn job posting URL
```

Test with real job posting or sample description:
```
Senior AI Engineer
Requirements:
- 5+ years Python experience
- LLMs and RAG expertise
- Cloud platforms (AWS/Azure)
- Team leadership experience
```

#### Test: profile-optimizer (Consolidation)
```
Input: Results from all other agents
Expected Output:
- Unified profile from all sources
- Cross-referenced data validation
- Comprehensive optimization plan
- No data conflicts
```

### 3. PARALLEL Pipeline Test

**Execute ALL tests simultaneously using single Task message:**

```xml
<!-- SEND ALL IN ONE MESSAGE FOR PARALLEL EXECUTION -->
<Task>
  <description>Test LinkedIn screenshot analysis</description>
  <prompt>Analyze all screenshots in candidate-data/linkedin/screenshots/ and validate extraction accuracy</prompt>
  <subagent_type>linkedin-specialist</subagent_type>
</Task>
<Task>
  <description>Test resume processing</description>
  <prompt>Process all files in candidate-data/resumes/resumes-files/ and validate multi-version handling</prompt>
  <subagent_type>resume-analyst</subagent_type>
</Task>
<Task>
  <description>Test job analysis</description>
  <prompt>Analyze sample job posting and validate requirement extraction</prompt>
  <subagent_type>job-offer-specialist</subagent_type>
</Task>
```

**Then after completion, run:**
```xml
<Task>
  <description>Test profile consolidation</description>
  <prompt>Consolidate all test results and validate data consistency</prompt>
  <subagent_type>profile-optimizer</subagent_type>
</Task>
```

### 4. Validation Checks

#### Screenshot Processing Validation
```python
checks = {
    "linkedin_screenshots_processed": True,
    "all_images_analyzed": True,
    "text_extracted_accurately": True,
    "no_hallucinations": True,
    "company_names_correct": True  # Attijariwafa not BforBank
}
```

#### Resume Processing Validation
```python
checks = {
    "all_resume_files_found": True,
    "pdf_files_processed": True,
    "docx_files_processed": True,
    "multiple_versions_compared": True,
    "auto_folder_scan_worked": True
}
```

#### Data Consistency Validation
```python
def validate_cross_source_consistency():
    """Check data consistency across sources."""
    linkedin_data = load("linkedin/profile-analysis.json")
    resume_data = load("resumes/resume-analysis.json")

    # Check name consistency
    assert linkedin_data["name"] == resume_data["name"]

    # Check experience dates alignment
    # Check skills overlap
    # Flag any discrepancies
```

### 5. Performance Metrics

Track parallel execution performance:
- Individual agent execution time
- Parallel vs sequential comparison
- Resource usage optimization
- Total pipeline duration

```json
{
  "performance": {
    "parallel_execution": {
      "linkedin_agent": "30s",
      "resume_agent": "25s",
      "job_agent": "20s",
      "total_parallel_time": "30s"  // Max of all
    },
    "sequential_baseline": "75s",
    "speedup": "2.5x",
    "cpu_utilization": "85%"
  }
}
```

### 6. Error Testing

Test error scenarios:
- No screenshots in folder
- No resume files
- Corrupted image files
- Invalid PDF files
- Mixed languages in resumes
- Partial screenshots

### 7. Test Report Generation

Generate comprehensive test report:
```json
{
  "test_summary": {
    "date": "2025-09-22",
    "mode": "parallel_execution",
    "data_sources": {
      "linkedin_screenshots": 5,
      "resume_files": 3,
      "test_jobs": 2
    }
  },
  "component_results": {
    "linkedin_specialist": {
      "status": "✅ PASS",
      "screenshots_processed": 5,
      "data_extracted": true,
      "no_hallucinations": true
    },
    "resume_analyst": {
      "status": "✅ PASS",
      "files_processed": 3,
      "auto_scan": true,
      "versions_compared": true
    },
    "job_offer_specialist": {
      "status": "✅ PASS",
      "requirements_extracted": true,
      "fit_score_calculated": true
    },
    "profile_optimizer": {
      "status": "✅ PASS",
      "data_consolidated": true,
      "conflicts_resolved": true
    }
  },
  "parallel_performance": {
    "execution_time": "30s",
    "speedup": "2.5x",
    "all_agents_parallel": true
  },
  "data_quality": {
    "accuracy": "98%",
    "completeness": "95%",
    "consistency": "100%"
  }
}
```

## Test Execution Commands

Based on argument provided:
- "linkedin" - Test LinkedIn screenshot analysis
- "resume" - Test resume file processing
- "job" - Test job analysis
- "optimizer" - Test profile consolidation
- "pipeline" - Test full parallel pipeline
- "all" - Run all tests with parallel execution

## Success Output

```
✅ Test Suite Complete (Parallel Mode)
- Screenshots Processed: 5/5
- Resume Files Analyzed: 3/3
- Parallel Execution: ENABLED
- Total Time: 30s (2.5x faster)
- Data Quality: 98% accurate

Key Validations:
✓ No hallucinations detected
✓ Company names correct (Attijariwafa)
✓ All files auto-detected
✓ Cross-source data consistent

Report: tests/logs/test-report-{date}.json
```

## Quick Test Command

To test the complete system with your data:
```
/test-system pipeline
```

This will:
1. Process all LinkedIn screenshots
2. Analyze all resume files
3. Test with sample job
4. **Run everything in parallel (single message, multiple Tasks)**
5. Generate comprehensive report

**Execution Time Comparison:**
- Sequential: ~15 minutes (one by one)
- Parallel: ~5 minutes (all at once)
- Speedup: 3x faster!