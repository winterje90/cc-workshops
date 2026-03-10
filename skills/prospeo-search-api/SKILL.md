---
name: prospeo-search-api
description: This skill should be used when searching for people/leads using the Prospeo Search Person API. It provides the correct API format, filter types, rate limiting patterns, and state-by-state crawling techniques to overcome the 25K result limit. Use when building lead lists, searching by job title/industry/location, or integrating with Prospeo.
---

# Prospeo Search Person API

This skill documents how to use the Prospeo Search Person API for finding leads with filters.

## When to Use

Use this skill when:
- Searching for people/leads by job title, industry, location, company size, etc.
- Building lead lists from Prospeo's database
- Running large US-wide searches that need state-by-state crawling

## API Overview

**Endpoint:** `POST https://api.prospeo.io/search-person`

**Authentication:** `X-KEY` header with API key

**Rate Limits:**
- 2-2.5 requests/second (120-150 req/min)
- Token bucket implementation recommended

**Result Limits:**
- 25 results per page
- 1000 pages max = 25,000 results per search
- 1 credit per search request that returns at least 1 result

## Request Format

```typescript
POST /search-person
Headers: {
  'Content-Type': 'application/json',
  'X-KEY': process.env.PROSPEO_API_KEY
}
Body: {
  page: number,        // 1-1000
  filters: ProspeoSearchFilters
}
```

## Filter Types

```typescript
interface ProspeoSearchFilters {
  // Location (use "State, United States #US" format)
  person_location_search?: {
    include?: string[];  // e.g., ["California, United States #US"]
    exclude?: string[];
  };

  // Job titles
  person_job_title?: {
    include?: string[];  // e.g., ["CEO", "Founder"]
    exclude?: string[];
    match_only_exact_job_titles?: boolean;
  };

  // Company size
  company_headcount_custom?: {
    min?: number;  // e.g., 11
    max?: number;  // e.g., 500
  };

  // Industry
  company_industry?: {
    include?: string[];  // e.g., ["Information Technology"]
    exclude?: string[];
  };

  // Technology stack
  company_technology?: {
    include?: string[];  // e.g., ["Salesforce", "HubSpot"]
    exclude?: string[];
  };

  // Contact requirements
  person_contact_details?: {
    email?: string[];   // ["VERIFIED"] for verified emails only
    mobile?: string[];
    operator?: string;
  };

  // Duplicate control
  person_duplicate_control?: {
    hide_people_from_all_my_lists?: boolean;
    hide_people_already_exported_before?: boolean;
  };

  // Company filters
  company_name?: { include?: string[]; exclude?: string[] };
  company_domain?: { include?: string[]; exclude?: string[] };
  company_revenue_custom?: { min?: number; max?: number };
  company_founding_year?: { min?: number; max?: number };
}
```

## Response Format

```typescript
interface ProspeoSearchApiResponse {
  error: boolean;
  message?: string;
  results?: ProspeoSearchResult[];
  pagination?: {
    current_page: number;
    total_page: number;
    total_count: number;
    per_page: number;  // Always 25
  };
}

interface ProspeoSearchResult {
  person: {
    person_id: string;
    first_name?: string;
    last_name?: string;
    full_name?: string;
    current_job_title?: string;
    linkedin_url?: string;
    email?: string;
    email_status?: string;
    phone?: string;
    location?: {
      city?: string;
      state?: string;
      country?: string;
    };
    job_history?: Array<{
      title?: string;
      company_name?: string;
      current?: boolean;
    }>;
  };
  company?: {
    company_id?: string;
    name?: string;
    domain?: string;
    linkedin_url?: string;
    industry?: string;
    headcount?: number;
    headcount_range?: string;
    technologies?: string[];
    location?: { city?: string; state?: string; country?: string };
  };
}
```

## State-by-State Crawling Pattern

For US-wide searches exceeding 25K results, split by state:

```typescript
const US_STATES_BY_SIZE = [
  'California', 'Texas', 'Florida', 'New York', 'Illinois', 'Pennsylvania',
  'Ohio', 'Georgia', 'North Carolina', 'Michigan', 'New Jersey', 'Virginia',
  'Washington', 'Arizona', 'Massachusetts', 'Tennessee', 'Indiana', 'Missouri',
  'Maryland', 'Wisconsin', 'Colorado', 'Minnesota', 'South Carolina', 'Alabama',
  'Louisiana', 'Kentucky', 'Oregon', 'Oklahoma', 'Connecticut', 'Utah', 'Iowa',
  'Nevada', 'Arkansas', 'Mississippi', 'Kansas', 'New Mexico', 'Nebraska',
  'Idaho', 'West Virginia', 'Hawaii', 'New Hampshire', 'Maine', 'Montana',
  'Rhode Island', 'Delaware', 'South Dakota', 'North Dakota', 'Alaska',
  'Vermont', 'Wyoming'
];

// Format for location filter
function formatStateLocation(state: string): string {
  return `${state}, United States #US`;
}

// Replace "United States #US" with state-specific location
function createStateFilters(baseFilters, state) {
  const stateFilters = JSON.parse(JSON.stringify(baseFilters));
  stateFilters.person_location_search.include =
    stateFilters.person_location_search.include.map(loc =>
      loc === 'United States #US' ? formatStateLocation(state) : loc
    );
  return stateFilters;
}
```

## Rate Limiting Implementation

```typescript
// Token bucket rate limiter
class TokenBucket {
  private tokens: number;
  private lastRefill: number;
  private maxTokens = 5;
  private refillRate = 2.0; // tokens per second

  async acquire(): Promise<void> {
    this.refill();
    if (this.tokens >= 1) {
      this.tokens -= 1;
      return;
    }
    const waitMs = Math.ceil(((1 - this.tokens) / this.refillRate) * 1000);
    await this.sleep(Math.max(waitMs, 500));
    this.refill();
    this.tokens -= 1;
  }
}
```

## Error Handling

```typescript
// Retry on 429 with exponential backoff
if (status === 429 && retryCount < 5) {
  const backoffMs = Math.min(2000 * Math.pow(2, retryCount), 60000);
  await sleep(backoffMs);
  return searchPeople(filters, page, retryCount + 1);
}
```

## Example: Search for Tech Executives

```typescript
const filters: ProspeoSearchFilters = {
  person_location_search: {
    include: ['United States #US']
  },
  person_job_title: {
    include: ['CEO', 'CTO', 'VP Engineering', 'Head of Engineering'],
    match_only_exact_job_titles: false
  },
  company_headcount_custom: {
    min: 11,
    max: 500
  },
  company_industry: {
    include: ['Information Technology', 'Software']
  },
  person_contact_details: {
    email: ['VERIFIED']
  }
};

const service = new ProspeoSearchService();
const { results, summary } = await service.searchWithStateSplitting(filters, {
  maxTotalContacts: 10000,
  maxContactsPerState: 5000
});
```

## Environment Variables

```bash
PROSPEO_API_KEY=your_api_key_here
```
