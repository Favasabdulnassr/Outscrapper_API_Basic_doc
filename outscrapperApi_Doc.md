#   Outscraper API – Basic Documentation


## 1. Purpose
Outscraper is a data extraction API that allows us to collect publicly available business information, including:

- Emails, phone numbers, social links (**Contacts & Leads**)  
- Business details from Google Maps (**Businesses & POI**)  
- Optional: reviews and comments 
- Company website information (**Company Website Finder**)  
- Product details from e-commerce sites (**Amazon Products API**)  
- Domain information and WHOIS details (**Domain Info**)  


---

## 2. Authentication

All API requests require an **API key**.

```

**Preferred (secure) method:**

X-API-KEY: YOUR_API_KEY

**Alternative (for testing only):**

?apiKey=YOUR_API_KEY


```


**Notes:**

- Keep API keys secure (environment variables or secret manager)  
- Never expose in frontend code or public repositories

---

## 3. Key Outscraper API Endpoints Overview

Below are some Outscraper API endpoints, their purposes, data returned, and key parameters.

| Endpoint URL | Purpose | Example Data Returned | Key Parameters | Required Parameters |
|--------------|----------|------------------------|----------------|---------------------|
| `https://api.outscraper.cloud/contacts-and-leads` | `Contacts & Leads - Finds emails, social links, phones, and other contacts from websites` | `query`, `details`, `emails`, `phones`, `site_data`, `socials`, `domain`, `contacts` | `query`, `preferredContacts`, `contactsPerCompany`, `emailsPerContact`, `generalEmails`, `fields`, `async`, `ui`, `format`, `webhook` | `query` |
| `https://api.outscraper.cloud/emails-and-contacts` | `Emails & Contacts - Finds email addresses, social links, and phones from domains` | `query`, `domain`, `emails`, `socials`, `site_data` | `query`, `preferredContacts`, `webhook` | `query` |
| `https://api.outscraper.cloud/google-maps-search` | Search businesses and points of interest (POI) on Google Maps in real-time. Supports batching, location-specific queries, and data enrichment. |  `name`, `full_address`, `phone`, `site`, `category`, `subtypes`, `rating`, `reviews`  | `query`, `limit`, `dropDuplicates`, `totalLimit`, `coordinates`, `skipPlaces`, `language`, `region`, `enrichment`, `fields`, `async`, `ui`, `format`, `webhook` | `query` |
| `https://api.outscraper.cloud/google-places-by-domain` | `Google Maps (Places) by Domain - Allows finding places by domains` | `query`, `name`, `full_address`, `phone`, `site`, `category`, `subtypes`, `rating`, `reviews`, `place_id`, `google_id`, `latitude`, `longitude` | `query`, `perQuery`, `region`, `fields`, `async`, `ui`, `format`, `webhook` | `query` |
| `https://api.outscraper.cloud/yelp-search` | `Yelp Search - Returns search results from Yelp` | `query`, `biz_id`, `name`, `price_range`, `rating`, `reviews`, `categories`, `formatted_address`, `neighborhoods`, `phone`, `business_url`, `services`, `photo`, `tags`, `snippet` | `query`, `limit`, `enrichment`, `fields`, `async`, `ui`, `format`, `webhook` | `query` |
| `https://api.outscraper.cloud/yelp-biz` | `Yelp Business - Returns detailed business information from Yelp` | `query`, `biz_id`, `name`, `business_url`, `closed`, `reviews`, `rating`, `image`, `phone`, `price_range`, `country`, `state`, `city`, `postal_code` | `query`, `fields`, `async`, `ui`, `format`, `webhook` | `query` |
| `https://api.outscraper.cloud/yellowpages-search` | `Yellow Pages Search - Returns search results from Yellow Pages` | `query`, `name`, `categories`, `position`, `years_in_business`, `years_with_yp`, `amenities`, `phone`, `street`, `locality`, `site`, `snippet`, `thumbnail`, `range`, `business_link` | `query`, `location`, `limit`, `region`, `enrichment`, `fields`, `async`, `ui`, `format`, `webhook` | `query` |
| `https://api.outscraper.cloud/company-website-finder` | `Finds company websites based on business names` | `query`, `fields`, `async`, `ui`, `format`, `webhook` | `query`, `fields`, `async`, `ui`, `format`, `webhook` | `query` |



---

## 4. Key Parameters (Contacts & Leads API)

- `query`: List of domains or URLs to scan  
- `preferredContacts`: Target roles. Supports multiple categories: "decision makers",         "influencers", "procurement/purchasing", "technical", "finance", "operations", "marketing",  "sales", "maintenance", "human resources", "legal and compliance", "supply chain/logistics", "education/training"
- `contactsPerCompany`: Number of contacts per company (default: 3)  
- `emailsPerContact`: Emails per contact (default: 1)  
- `generalEmails`: `true` = only info@/support@ type, `false` = personal emails  
- `async`: `true` = background processing (default), `false` = wait for result immediately  
- `fields`: Optional, select which fields to return (name, email, phone, etc.)  
- `format`: Output format: json, csv, xlsx, parquet  
- `webhook`: Optional callback URL for async jobs  

---

## 5. Python Examples

### 5.1 Direct API request (using `requests`)
```python
import requests
import os

API_KEY = os.getenv("OUTSCRAPER_API_KEY")
url = "https://api.outscraper.cloud/contacts-and-leads"
headers = {"X-API-KEY": API_KEY}

params = {
    "query": ["outscraper.com", "examplelawfirm.com"],
    "preferredContacts": ["marketing", "sales"],
    "contactsPerCompany": 3,
    "emailsPerContact": 1,
    "generalEmails": False,
    "async": False,
    "format": ["json"]
}

response = requests.get(url, headers=headers, params=params)
data = response.json()
print(data)


```


### 5.2 Using the official Outscraper Python SDK

```python


from outscraper import ApiClient

api_client = ApiClient(api_key='YOUR-API-KEY')
results = api_client.contacts_and_leads(['outscraper.com', 'examplelawfirm.com'])


print(results)


```

## 6. Sample Response

```python


[
  {
    "query": "examplelawfirm.com",
    "contacts": [
      {
        "name": "John Doe",
        "position": "Marketing Manager",
        "email": "john@examplelawfirm.com",
        "phone": "+1 555 123 4567",
        "linkedin": "https://linkedin.com/in/johndoe"
      }
    ]
  }
]


```

## 7. Conclusion / Notes

- Outscraper APIs provide an easy way to extract contacts, emails, and business information from websites and Google Maps.  
- API key authentication is required for all requests, passed via `X-API-KEY` header.    
- Start with **Contacts & Leads** and **Businesses & POI** endpoints, then explore other endpoints as needed.  
- Always store API keys securely and follow best practices for production code.

