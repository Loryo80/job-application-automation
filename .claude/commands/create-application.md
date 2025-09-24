---
allowed-tools: Task, Read, Write, MultiEdit
argument-hint: [job-analysis-file or job-url]
description: Create complete application package with resume and cover letter
model: opus
---

# Complete Application Package Generator

Generate tailored application materials based on job analysis and candidate profile.

## Input
Job Analysis: $ARGUMENTS

## Generation Pipeline

### Phase 1: Load Data
1. Load job analysis from `job-offers/{company}/analysis.json`
2. Load candidate profile from `candidate-data/profile.json`
3. Load optimization suggestions from `candidate-data/optimization-plan.json`

### Phase 2: Resume Customization
Create tailored resume:
- Select relevant experiences
- Rewrite bullets with job keywords
- Optimize section ordering
- Ensure ATS compatibility
- Target length: 1-2 pages

Key customizations:
- Use action verbs from job posting
- Include 70%+ of required keywords
- Quantify all achievements
- Match job's tone and style

### Phase 3: Cover Letter Creation
Generate compelling cover letter:

**Structure:**
1. **Hook Opening** - Connection to company/role
2. **Value Proposition** - Top 3 relevant achievements
3. **Culture Fit** - Alignment with company values
4. **Call to Action** - Next steps enthusiasm

**Key Elements:**
- Address specific requirements
- Show company research
- Demonstrate cultural fit
- Include unique differentiators

### Phase 4: Application Checklist
Generate checklist:
- [ ] Resume customized with keywords
- [ ] Cover letter personalized
- [ ] LinkedIn profile updated
- [ ] Portfolio/GitHub ready
- [ ] References prepared
- [ ] Follow-up calendar set

## Output Files

### 1. Tailored Resume
Save to: `job-offers/{company}/resume-{date}.docx`

### 2. Cover Letter
Save to: `job-offers/{company}/cover-letter-{date}.docx`

### 3. Application Tracker
Update: `job-offers/application-tracker.json`
```json
{
  "company": "",
  "position": "",
  "applied_date": "",
  "customizations": [],
  "follow_up_dates": []
}
```

### 4. Email Template
Save to: `job-offers/{company}/email-template.txt`
```
Subject: {Position} Application - {Your Name}

Dear {Hiring Manager},

{Personalized opening referencing company/role}

Please find attached my resume and cover letter for the {Position} role.

{Brief value proposition - 2 sentences}

Thank you for considering my application. I look forward to discussing how I can contribute to {Company}.

Best regards,
{Your Name}
```

## Quality Assurance
- ATS score: Must be 80+
- Keyword density: 70%+ match
- Grammar check: Zero errors
- Format verification: Professional
- Length check: Within limits