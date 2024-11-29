# **Elasticsearch Full-Text Search Example**

## **Sample Data**
The dataset contains articles with the following structure:

```json
[
  {
    "title": "Elasticsearch Basics",
    "content": "Elasticsearch is a powerful search engine based on the Lucene library.",
    "author": "John Doe",
    "tags": ["elasticsearch", "search", "basics"],
    "publication_date": "2024-01-15"
  },
  {
    "title": "Advanced Elasticsearch Queries",
    "content": "Learn about aggregations, filters, and full-text search in Elasticsearch.",
    "author": "Jane Smith",
    "tags": ["elasticsearch", "advanced", "queries"],
    "publication_date": "2024-03-10"
  },
  {
    "title": "Introduction to Search Engines",
    "content": "Search engines like Elasticsearch and Solr are essential for handling large datasets.",
    "author": "Alice Johnson",
    "tags": ["search", "elasticsearch", "solr"],
    "publication_date": "2023-12-01"
  }
]
```

---

## **Indexing Data**

Index the above data using the `_bulk` API:

```bash
POST /articles/_bulk
{ "index": { "_id": 1 } }
{ "title": "Elasticsearch Basics", "content": "Elasticsearch is a powerful search engine based on the Lucene library.", "author": "John Doe", "tags": ["elasticsearch", "search", "basics"], "publication_date": "2024-01-15" }
{ "index": { "_id": 2 } }
{ "title": "Advanced Elasticsearch Queries", "content": "Learn about aggregations, filters, and full-text search in Elasticsearch.", "author": "Jane Smith", "tags": ["elasticsearch", "advanced", "queries"], "publication_date": "2024-03-10" }
{ "index": { "_id": 3 } }
{ "title": "Introduction to Search Engines", "content": "Search engines like Elasticsearch and Solr are essential for handling large datasets.", "author": "Alice Johnson", "tags": ["search", "elasticsearch", "solr"], "publication_date": "2023-12-01" }
```

---

## **Queries**

### **1. Match Query**
Search for the term "search engine" in the `content` field.
```json
GET /articles/_search
{
  "query": {
    "match": {
      "content": "search engine"
    }
  }
}
```

---

### **2. Match Query with Boost**
Boost the relevance of documents with the term "elasticsearch" in the `tags` field.
```json
GET /articles/_search
{
  "query": {
    "match": {
      "tags": {
        "query": "elasticsearch",
        "boost": 2
      }
    }
  }
}
```

---

### **3. Multi-Match Query**
Search across `title` and `content` fields.
```json
GET /articles/_search
{
  "query": {
    "multi_match": {
      "query": "search engine",
      "fields": ["title", "content"]
    }
  }
}
```

---

### **4. Phrase Match Query**
Search for the exact phrase "full-text search".
```json
GET /articles/_search
{
  "query": {
    "match_phrase": {
      "content": "full-text search"
    }
  }
}
```

---

### **5. Bool Query**
Combine multiple conditions:
- `content` must include "search".
- `tags` should include "elasticsearch".
- Exclude articles with the tag "advanced".
```json
GET /articles/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "content": "search" } }
      ],
      "filter": [
        { "term": { "tags": "elasticsearch" } }
      ],
      "must_not": [
        { "term": { "tags": "advanced" } }
      ]
    }
  }
}
```

---

### **6. Highlighting**
Highlight matching terms in the `content` field.
```json
GET /articles/_search
{
  "query": {
    "match": {
      "content": "Elasticsearch"
    }
  },
  "highlight": {
    "fields": {
      "content": {}
    }
  }
}
```

---

### **7. Fuzzy Query**
Handle typos in the search term (e.g., "elesticsearch").
```json
GET /articles/_search
{
  "query": {
    "match": {
      "content": {
        "query": "elesticsearch",
        "fuzziness": "AUTO"
      }
    }
  }
}
```

---

### **8. Wildcard Query**
Search for terms starting with "Elast*".
```json
GET /articles/_search
{
  "query": {
    "wildcard": {
      "title": "Elast*"
    }
  }
}
```

---

### **9. Range Query**
Find articles published between `2023-01-01` and `2024-01-01`.
```json
GET /articles/_search
{
  "query": {
    "range": {
      "publication_date": {
        "gte": "2023-01-01",
        "lte": "2024-01-01"
      }
    }
  }
}
```

---

### **10. Aggregations**
Group articles by author.
```json
GET /articles/_search
{
  "size": 0,
  "aggs": {
    "authors": {
      "terms": {
        "field": "author.keyword"
      }
    }
  }
}
```

---