
# Action Plan for AI Interviewer Agent Data Pipeline

## 1. Sample Target MongoDB Schema (`questions` collection)

```json
{
  "question_id": "BHV-LEAD-001",
  "question_text": "Tell me about a time your behavior had a positive impact on your team.",
  "question_type": "Behavioral",
  "competencies": ["Leadership", "Team Collaboration"],
  "metadata": {
    "source_dataset": "Kaggle: HR Questions",
    "job_roles": ["Software Engineer", "Product Manager"],
    "experience_level": "Intermediate",
    "difficulty": "Medium",
    "follow_up_questions": ["What was the specific impact?", "How did you measure success?"]
  },
  "rubric": {
    "rating_scale": [
      {"score": 1, "label": "Unsatisfactory"},
      {"score": 2, "label": "Satisfactory"},
      {"score": 3, "label": "Average"},
      {"score": 4, "label": "Above Average"},
      {"score": 5, "label": "Exceptional"}
    ],
    "evaluation_criteria": [
      {
        "competency": "Leadership",
        "description": "Assesses ability to influence and guide a team",
        "anchors": {
          "5": "Candidate clearly describes proactive leadership action with quantifiable positive outcome...",
          "3": "Candidate describes positive contribution but lacks specific ownership or measurable impact...",
          "1": "Fails to provide a relevant example or demonstrates negative impact..."
        }
      }
    ]
  }
}
```

## 2. Priority Data Sources

**Tier 1 (Primary Content):** Kaggle's "HR Interview Questions and Ideal Answers" (250k questions, JSON format)
- Fields: `question_text`, `ideal_answer`, `job_roles`, `question_type`, `difficulty_level`

**Tier 2 (Rubric Blueprint):** Exponent's ML Coding & Concepts Rubrics
- Provides multi-level rating scales with behavioral anchors
- Use as template for rubric structure

**Tier 3 (Supplemental):** Specialized question sets (GitHub repos for data science, software engineering, etc.)

## 3. Data Augmentation Process

Use LLM to generate rubric anchors from ideal answers:

```python
# Prompt template for anchor generation
prompt_template = """
Role: You are an expert HR manager creating a scoring rubric.
Context:
Interview Question: "{question_text}"
Core Competency: "{competency}"
Excellent Response (Score 5): "{excellent_answer}"

Task: Write a behavioral anchor for a '{target_level}' (Score {target_score}) response.
Focus on observable characteristics compared to the excellent response.
"""
```

## 4. Implementation Steps

1. **Ingest Tier 1 dataset** into staging environment
2. **Normalize schema** to match target MongoDB structure
3. **Map competencies** from source metadata to standardized list
4. **Seed "Excellent" anchors** from ideal_answer field
5. **Generate intermediate anchors** using LLM with above prompt template
6. **Populate MongoDB** with augmented documents
7. **Create indexes** on: `question_type`, `competencies`, `metadata.job_roles`, `metadata.experience_level`, `metadata.difficulty`

## 5. Query Patterns for Agent

```javascript
// Question selection query
db.questions.find({
  "metadata.job_roles": "Software Engineer",
  "metadata.experience_level": "Senior",
  "question_type": "Behavioral",
  "competencies": "Leadership",
  "metadata.difficulty": "Medium"
}).limit(1)

// Rubric retrieval query
db.questions.findOne({"_id": ObjectId("...")})
```

## 6. Additional Collections Needed

- `candidate_sessions`: Store interview transcripts and context
- `evaluations`: Store agent's assessments for continuous improvement

## 7. Immediate Actions

1. Acquire Kaggle dataset
2. Set up MongoDB instance with proposed schema
3. Develop data ingestion pipeline
4. Implement LLM integration for rubric generation
5. Create indexing strategy
6. Build query interface for agent integration
```
