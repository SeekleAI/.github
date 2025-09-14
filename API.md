# SeekleAI API Documentation

## API Overview

The SeekleAI API provides programmatic access to our AI-powered lead generation platform. Use these endpoints to configure your ICP, retrieve qualified leads, and manage your lead generation campaigns.

## Base URL
```
https://api.seekleai.com/v1
```

## Authentication
All API requests require authentication using an API key:
```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
     -H "Content-Type: application/json" \
     "https://api.seekleai.com/v1/leads"
```

## Endpoints

### ICP Configuration

#### Create/Update ICP
```http
POST /icp
Content-Type: application/json

{
  "name": "Enterprise SaaS Companies",
  "criteria": {
    "company_size": {
      "min_employees": 100,
      "max_employees": 5000
    },
    "industries": ["Software", "SaaS", "Technology"],
    "locations": ["United States", "Canada", "United Kingdom"],
    "revenue_range": {
      "min": 10000000,
      "max": 500000000
    },
    "technologies": ["Salesforce", "HubSpot", "AWS"],
    "signals": {
      "growth_indicators": ["funding", "expansion", "hiring"],
      "intent_signals": ["technology_research", "competitor_evaluation"],
      "timing_signals": ["budget_cycle", "contract_renewal"]
    }
  },
  "scoring_weights": {
    "fit_score": 0.4,
    "intent_score": 0.3,
    "timing_score": 0.2,
    "contact_score": 0.1
  }
}
```

#### Get ICP Configuration
```http
GET /icp/{icp_id}
```

### Lead Management

#### Get Qualified Leads
```http
GET /leads?icp_id={icp_id}&limit=50&offset=0&min_score=75

Response:
{
  "leads": [
    {
      "id": "lead_12345",
      "company": {
        "name": "TechCorp Inc.",
        "domain": "techcorp.com",
        "industry": "Software",
        "size": 250,
        "location": "San Francisco, CA",
        "revenue_estimate": 50000000
      },
      "scores": {
        "overall": 87,
        "fit": 92,
        "intent": 78,
        "timing": 85,
        "contact": 95
      },
      "signals": [
        {
          "type": "growth_signal",
          "source": "linkedin",
          "description": "40% employee growth in last 6 months",
          "confidence": 0.95,
          "detected_at": "2024-01-15T10:30:00Z"
        },
        {
          "type": "intent_signal",
          "source": "news",
          "description": "CEO mentioned digital transformation initiative",
          "confidence": 0.87,
          "detected_at": "2024-01-14T14:22:00Z"
        }
      ],
      "contacts": [
        {
          "name": "John Smith",
          "title": "VP of Sales",
          "email": "john.smith@techcorp.com",
          "linkedin": "https://linkedin.com/in/johnsmith",
          "confidence": 0.92
        }
      ],
      "recommendations": {
        "engagement_strategy": "Direct outreach focusing on scaling challenges",
        "best_contact": "john.smith@techcorp.com",
        "timing": "Immediate - high intent signals detected",
        "messaging_angle": "Digital transformation and sales efficiency"
      },
      "last_updated": "2024-01-15T12:45:00Z"
    }
  ],
  "pagination": {
    "total": 1247,
    "limit": 50,
    "offset": 0,
    "has_more": true
  }
}
```

#### Get Lead Details
```http
GET /leads/{lead_id}
```

#### Update Lead Status
```http
PATCH /leads/{lead_id}
Content-Type: application/json

{
  "status": "contacted",
  "notes": "Initial outreach sent via email",
  "feedback": {
    "quality_rating": 4,
    "conversion_likelihood": "high"
  }
}
```

### Signal Monitoring

#### Get Recent Signals
```http
GET /signals?company_domain=techcorp.com&days=7

Response:
{
  "signals": [
    {
      "id": "signal_789",
      "type": "hiring_signal",
      "source": "naukri",
      "company": "TechCorp Inc.",
      "description": "Posted 5 new sales positions",
      "confidence": 0.88,
      "relevance_score": 85,
      "detected_at": "2024-01-15T09:15:00Z",
      "metadata": {
        "job_titles": ["Sales Manager", "Account Executive", "SDR"],
        "department": "Sales",
        "urgency": "high"
      }
    }
  ]
}
```

#### Subscribe to Signal Alerts
```http
POST /signals/subscriptions
Content-Type: application/json

{
  "icp_id": "icp_123",
  "signal_types": ["funding", "expansion", "technology_adoption"],
  "webhook_url": "https://your-app.com/webhooks/signals",
  "email_notifications": true,
  "minimum_confidence": 0.8
}
```

### Analytics and Reporting

#### Get Campaign Performance
```http
GET /analytics/performance?icp_id={icp_id}&period=30d

Response:
{
  "period": {
    "start": "2023-12-16",
    "end": "2024-01-15"
  },
  "metrics": {
    "leads_generated": 342,
    "leads_contacted": 156,
    "responses_received": 47,
    "meetings_scheduled": 23,
    "deals_closed": 8,
    "conversion_rate": 0.30,
    "average_lead_score": 78.5,
    "data_sources_breakdown": {
      "linkedin": 0.35,
      "news": 0.25,
      "forums": 0.15,
      "reddit": 0.12,
      "naukri": 0.08,
      "mca21": 0.05
    }
  }
}
```

## Webhooks

### Signal Alerts
When high-value signals are detected, we can send real-time notifications to your webhook endpoint:

```json
{
  "event": "signal_detected",
  "timestamp": "2024-01-15T10:30:00Z",
  "data": {
    "signal_id": "signal_123",
    "company": {
      "name": "TechCorp Inc.",
      "domain": "techcorp.com"
    },
    "signal": {
      "type": "funding_round",
      "description": "Series B funding of $25M announced",
      "confidence": 0.95,
      "source": "news"
    },
    "lead_score_update": {
      "previous_score": 75,
      "new_score": 89,
      "score_change": 14
    }
  }
}
```

### New Qualified Leads
```json
{
  "event": "new_qualified_lead",
  "timestamp": "2024-01-15T11:00:00Z",
  "data": {
    "lead_id": "lead_456",
    "company": {
      "name": "InnovateCorp",
      "domain": "innovatecorp.com"
    },
    "overall_score": 92,
    "icp_match": "enterprise_saas",
    "key_signals": [
      "rapid_growth",
      "technology_adoption",
      "hiring_spree"
    ]
  }
}
```

## Rate Limits

- **Standard Plan**: 1000 requests/hour
- **Professional Plan**: 5000 requests/hour  
- **Enterprise Plan**: 20000 requests/hour

Rate limit headers are included in all responses:
```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1642694400
```

## Error Handling

The API uses standard HTTP status codes:

- `200` - Success
- `400` - Bad Request (invalid parameters)
- `401` - Unauthorized (invalid API key)
- `403` - Forbidden (insufficient permissions)
- `404` - Not Found
- `429` - Too Many Requests (rate limit exceeded)
- `500` - Internal Server Error

Error responses include details:
```json
{
  "error": {
    "code": "INVALID_ICP_CRITERIA",
    "message": "The specified industry 'InvalidIndustry' is not supported",
    "details": {
      "supported_industries": ["Software", "SaaS", "Technology", "Healthcare", "Finance"]
    }
  }
}
```

## SDKs and Libraries

We provide official SDKs for popular programming languages:

- **Python**: `pip install seekleai-python`
- **JavaScript/Node.js**: `npm install seekleai-js`
- **Java**: Available via Maven Central
- **Go**: `go get github.com/seekleai/go-sdk`

Example usage with Python SDK:
```python
from seekleai import Client

client = Client(api_key='your_api_key')

# Get qualified leads
leads = client.leads.list(icp_id='icp_123', min_score=80)

for lead in leads:
    print(f"Company: {lead.company.name}, Score: {lead.scores.overall}")
```

## Support

- **Documentation**: [https://docs.seekleai.com](https://docs.seekleai.com)
- **Support Email**: api-support@seekleai.com
- **Status Page**: [https://status.seekleai.com](https://status.seekleai.com)
- **Community**: [https://community.seekleai.com](https://community.seekleai.com)