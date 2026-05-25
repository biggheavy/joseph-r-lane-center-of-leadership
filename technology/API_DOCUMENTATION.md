# API Documentation

## Base URL
```
https://api.josephrlanecenter.com/v1
```

## Authentication

### API Key
```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
     https://api.josephrlanecenter.com/v1/courses
```

### OAuth 2.0
```python
from requests_oauthlib import OAuth2Session

client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"

oauth = OAuth2Session(client_id)
token = oauth.fetch_token(
    'https://api.josephrlanecenter.com/oauth/token',
    client_secret=client_secret
)
```

---

## AI Endpoints

### 1. Generate Learning Path

**POST** `/ai/learning-path`

Generate personalized learning path for a student.

**Request:**
```json
{
  "user_id": "user_123",
  "career_goals": ["Become a manager", "Leadership development"],
  "current_skills": ["Communication", "Problem solving"],
  "available_hours_per_week": 10,
  "learning_style": "visual",
  "experience_level": "intermediate"
}
```

**Response (200 OK):**
```json
{
  "learning_path_id": "lp_456",
  "recommended_courses": [
    {
      "course_id": "course_001",
      "course_name": "Advanced Leadership",
      "sequence": 1,
      "start_date": "2026-06-01",
      "end_date": "2026-06-15",
      "compatibility_score": 0.92
    },
    {
      "course_id": "course_002",
      "course_name": "Team Management",
      "sequence": 2,
      "start_date": "2026-06-16",
      "end_date": "2026-06-30"
    }
  ],
  "estimated_duration_months": 3,
  "success_probability": 0.87,
  "created_at": "2026-05-25T10:30:00Z"
}
```

**Errors:**
- `400 Bad Request`: Invalid parameters
- `401 Unauthorized`: Invalid API key
- `422 Unprocessable Entity`: Missing required fields

---

### 2. Get Recommendations

**GET** `/ai/recommendations?user_id={user_id}&limit=5`

Get course recommendations for a user.

**Response (200 OK):**
```json
{
  "recommendations": [
    {
      "course_id": "course_001",
      "title": "Strategic Leadership",
      "description": "...",
      "difficulty": 4,
      "rating": 4.8,
      "enrollment_count": 1250,
      "match_score": 0.95,
      "reason": "Matches your leadership goals and skill level"
    }
  ]
}
```

---

### 3. Predict Student Success

**POST** `/ai/predict-success`

Predict probability of course completion.

**Request:**
```json
{
  "user_id": "user_123",
  "course_id": "course_001",
  "historical_metrics": {
    "completion_rate": 0.85,
    "average_score": 82,
    "hours_committed_per_week": 10
  }
}
```

**Response (200 OK):**
```json
{
  "success_probability": 0.87,
  "risk_level": "low",
  "recommendations": [
    "Maintain consistent weekly study schedule",
    "Participate in discussion forums",
    "Complete all assignments on time"
  ]
}
```

---

### 4. AI Analytics

**GET** `/ai/analytics?user_id={user_id}&metric_type=learning_progress`

Get AI-generated analytics and insights.

**Response (200 OK):**
```json
{
  "user_id": "user_123",
  "metrics": {
    "learning_velocity": 1.2,
    "skill_growth_rate": 0.15,
    "knowledge_retention": 0.82,
    "estimated_mastery_date": "2026-08-15"
  },
  "insights": [
    "Your learning speed is 20% above average",
    "Strong performance in practical modules",
    "Consider additional challenge courses"
  ]
}
```

---

## Quantum Computing Endpoints

### 1. Schedule Optimization

**POST** `/quantum/optimize-schedule`

Optimize course schedule using quantum computing.

**Request:**
```json
{
  "courses": [
    {
      "id": "course_001",
      "name": "Leadership 101",
      "duration": 60,
      "min_class_size": 20,
      "max_class_size": 50
    }
  ],
  "instructors": [
    {
      "id": "instr_001",
      "name": "Dr. Smith",
      "available_hours_per_week": 15
    }
  ],
  "constraints": {
    "no_room_conflicts": true,
    "max_classes_per_instructor_per_day": 3
  }
}
```

**Response (200 OK):**
```json
{
  "optimization_id": "opt_789",
  "schedule": [
    {
      "course_id": "course_001",
      "instructor_id": "instr_001",
      "day": "Monday",
      "time": "09:00",
      "room": "A101",
      "duration_minutes": 60
    }
  ],
  "fitness_score": 98.5,
  "optimization_method": "hybrid_classical_quantum",
  "computation_time_ms": 2340
}
```

---

### 2. Resource Allocation

**POST** `/quantum/optimize-resources`

Optimize resource allocation using quantum algorithms.

**Request:**
```json
{
  "resources": [
    {
      "id": "res_001",
      "name": "Laptop",
      "quantity": 50,
      "cost": 1000,
      "allocation": [
        {"department": "AI", "needed": 20},
        {"department": "Quantum", "needed": 15}
      ]
    }
  ],
  "budget": 100000
}
```

**Response (200 OK):**
```json
{
  "allocation": {
    "department_ai": 20,
    "department_quantum": 15,
    "department_data": 10
  },
  "total_cost": 45000,
  "remaining_budget": 55000,
  "efficiency_score": 0.92
}
```

---

### 3. Quantum Job Status

**GET** `/quantum/jobs/{job_id}`

Check status of quantum computation job.

**Response (200 OK):**
```json
{
  "job_id": "job_123",
  "status": "completed",
  "submitted_at": "2026-05-25T10:00:00Z",
  "completed_at": "2026-05-25T10:15:30Z",
  "execution_time_ms": 930,
  "backend": "ibm_brisbane",
  "result": {
    "optimization_found": true,
    "quality_score": 0.98
  }
}
```

**Status Values:**
- `queued`: Waiting to execute
- `running`: Currently executing
- `completed`: Successfully completed
- `failed`: Execution failed

---

## User Management Endpoints

### 1. Get User Profile

**GET** `/users/{user_id}`

**Response (200 OK):**
```json
{
  "user_id": "user_123",
  "name": "John Smith",
  "email": "john@example.com",
  "enrollment_date": "2026-01-15",
  "total_hours_completed": 125,
  "current_courses": 3,
  "certifications": ["Leadership Basics", "Team Management"],
  "learning_profile": {
    "learning_style": "visual",
    "pace": "moderate",
    "skill_level": 3
  }
}
```

### 2. Update User Profile

**PUT** `/users/{user_id}`

**Request:**
```json
{
  "name": "John Smith",
  "learning_preferences": {
    "preferred_format": "video",
    "timezone": "EST",
    "notification_frequency": "daily"
  }
}
```

---

## Course Management Endpoints

### 1. List Courses

**GET** `/courses?category=leadership&difficulty=3&limit=20`

**Response (200 OK):**
```json
{
  "data": [
    {
      "course_id": "course_001",
      "title": "Leadership 101",
      "category": "leadership",
      "difficulty": 2,
      "duration_hours": 20,
      "instructor": "Dr. Smith",
      "enrollment": 1250,
      "rating": 4.8
    }
  ],
  "total": 150,
  "page": 1,
  "limit": 20
}
```

### 2. Get Course Details

**GET** `/courses/{course_id}`

**Response (200 OK):**
```json
{
  "course_id": "course_001",
  "title": "Advanced Leadership",
  "description": "Master advanced leadership techniques...",
  "syllabus": [
    {
      "week": 1,
      "topic": "Strategic Vision",
      "lessons": 5
    }
  ],
  "instructor": {
    "id": "instr_001",
    "name": "Dr. Smith",
    "bio": "..."
  },
  "prerequisites": ["Leadership Basics"],
  "certification_available": true
}
```

### 3. Enroll in Course

**POST** `/courses/{course_id}/enroll`

**Request:**
```json
{
  "user_id": "user_123"
}
```

**Response (201 Created):**
```json
{
  "enrollment_id": "enr_456",
  "course_id": "course_001",
  "user_id": "user_123",
  "enrolled_at": "2026-05-25T10:30:00Z",
  "access_expires": "2026-08-25T23:59:59Z"
}
```

---

## Assessment Endpoints

### 1. Submit Assessment

**POST** `/assessments/{assessment_id}/submit`

**Request:**
```json
{
  "user_id": "user_123",
  "answers": [
    {"question_id": "q_1", "answer": "A"},
    {"question_id": "q_2", "answer": "B"}
  ]
}
```

**Response (201 Created):**
```json
{
  "submission_id": "sub_789",
  "score": 85,
  "grade": "A",
  "feedback": "Excellent work!",
  "submitted_at": "2026-05-25T11:20:00Z"
}
```

### 2. Get Assessment Results

**GET** `/assessments/{assessment_id}/results?user_id={user_id}`

**Response (200 OK):**
```json
{
  "assessment_id": "assess_001",
  "user_id": "user_123",
  "submission_id": "sub_789",
  "score": 85,
  "max_score": 100,
  "percentage": 85,
  "grade": "A",
  "submitted_at": "2026-05-25T11:20:00Z",
  "graded_at": "2026-05-25T11:25:00Z"
}
```

---

## Progress Tracking Endpoints

### 1. Get User Progress

**GET** `/users/{user_id}/progress`

**Response (200 OK):**
```json
{
  "user_id": "user_123",
  "overall_completion": 0.65,
  "courses": [
    {
      "course_id": "course_001",
      "title": "Leadership 101",
      "progress": 1.0,
      "status": "completed",
      "completion_date": "2026-05-20"
    },
    {
      "course_id": "course_002",
      "title": "Team Management",
      "progress": 0.45,
      "status": "in_progress",
      "estimated_completion": "2026-06-10"
    }
  ]
}
```

### 2. Update Course Progress

**PUT** `/courses/{course_id}/progress`

**Request:**
```json
{
  "user_id": "user_123",
  "current_lesson": 5,
  "completion_percentage": 45,
  "time_spent_minutes": 180
}
```

---

## Certification Endpoints

### 1. List User Certifications

**GET** `/users/{user_id}/certifications`

**Response (200 OK):**
```json
{
  "certifications": [
    {
      "cert_id": "cert_001",
      "name": "Leadership Fundamentals",
      "issued_date": "2026-05-15",
      "expiry_date": "2027-05-15",
      "status": "active",
      "verification_url": "https://verify.josephrlanecenter.com/cert_001"
    }
  ]
}
```

---

## Error Responses

### Standard Error Format
```json
{
  "error": {
    "code": "INVALID_REQUEST",
    "message": "Invalid parameters provided",
    "details": {
      "field": "user_id",
      "issue": "Required field missing"
    }
  }
}
```

### Error Codes
- `INVALID_REQUEST` (400): Bad request
- `UNAUTHORIZED` (401): Authentication failed
- `FORBIDDEN` (403): Access denied
- `NOT_FOUND` (404): Resource not found
- `CONFLICT` (409): Resource conflict
- `RATE_LIMIT` (429): Too many requests
- `SERVER_ERROR` (500): Internal server error

---

## Rate Limiting

```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1374284313
```

**Limits:**
- Free tier: 100 requests/hour
- Pro tier: 10,000 requests/hour
- Enterprise: Custom limits

---

## Webhooks

### Course Completion Event
```json
{
  "event_type": "course.completed",
  "timestamp": "2026-05-25T12:00:00Z",
  "data": {
    "user_id": "user_123",
    "course_id": "course_001",
    "final_score": 88,
    "completion_date": "2026-05-25"
  }
}
```

### Student Dropout Risk Alert
```json
{
  "event_type": "student.at_risk",
  "timestamp": "2026-05-25T12:00:00Z",
  "data": {
    "user_id": "user_456",
    "risk_level": "high",
    "last_activity": "2026-05-10",
    "reason": "Missing assignments"
  }
}
```

---

## Code Examples

### Python
```python
import requests

API_KEY = "your_api_key"
BASE_URL = "https://api.josephrlanecenter.com/v1"

headers = {"Authorization": f"Bearer {API_KEY}"}

# Get learning path
response = requests.post(
    f"{BASE_URL}/ai/learning-path",
    headers=headers,
    json={
        "user_id": "user_123",
        "career_goals": ["Leadership"],
        "available_hours_per_week": 10
    }
)

learning_path = response.json()
print(learning_path)
```

### JavaScript
```javascript
const API_KEY = "your_api_key";
const BASE_URL = "https://api.josephrlanecenter.com/v1";

async function getRecommendations(userId) {
  const response = await fetch(
    `${BASE_URL}/ai/recommendations?user_id=${userId}`,
    {
      headers: {
        "Authorization": `Bearer ${API_KEY}`
      }
    }
  );
  
  return await response.json();
}
```

### cURL
```bash
curl -X POST https://api.josephrlanecenter.com/v1/ai/learning-path \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "user_id": "user_123",
    "career_goals": ["Leadership"],
    "available_hours_per_week": 10
  }'
```

---

## API Versioning

Current version: **v1**

**Breaking Changes Policy:**
- Major versions (v2, v3): Breaking changes allowed
- Minor versions within major: Backward compatible
- Deprecated endpoints: 6-month notice before removal

---

## Support

- **Documentation:** https://docs.josephrlanecenter.com
- **Issues:** https://github.com/josephrlane/issues
- **Email:** api-support@josephrlanecenter.com
- **Slack:** [Join community](https://slack.josephrlanecenter.com)
