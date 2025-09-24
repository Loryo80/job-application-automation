---
allowed-tools: Task, WebFetch, Write, Read, mcp__playwright__navigate, mcp__playwright__screenshot
argument-hint: [job-url or company/position]
description: Analyze job posting and calculate fit score
model: opus
---

# Job Opportunity Analysis & Scoring

Analyze the job posting provided and generate comprehensive insights with fit scoring.

## Input
Job Reference: $ARGUMENTS

## Analysis Process

### Step 1: Data Extraction
Use the job-offer-specialist subagent to:
- Extract all job requirements
- Identify key skills and qualifications
- Analyze company culture indicators
- Determine compensation range
- Calculate market competitiveness

### Step 2: Candidate Matching
Load candidate profile from `candidate-data/profile.json` and:
- Match skills against requirements
- Compare experience levels
- Assess culture fit
- Calculate realistic fit score (0-100)

### Step 3: Strategic Analysis
Generate application strategy:
- Customization priorities
- Keywords to emphasize
- Experiences to highlight
- Potential concerns to address

### Step 4: Competitive Intelligence
Research and analyze:
- Similar positions in market
- Company hiring patterns
- Team composition (via LinkedIn)
- Recent company news and developments

## Output Structure

Save analysis to: `job-offers/{company-name}/analysis-{date}.json`

```json
{
  "job_summary": {},
  "fit_analysis": {
    "overall_score": 0,
    "strengths": [],
    "gaps": [],
    "recommendations": []
  },
  "application_strategy": {},
  "market_intelligence": {}
}
```

## Decision Framework
- Score 80+: Priority application
- Score 60-79: Strong candidate
- Score 40-59: Stretch opportunity
- Score <40: Reconsider applying

Generate actionable next steps based on score.