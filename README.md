<div align="center">

```
 ████████╗██╗██╗  ██╗████████╗ ██████╗ ██╗  ██╗
    ██╔══╝██║██║ ██╔╝╚══██╔══╝██╔═══██╗██║ ██╔╝
    ██║   ██║█████╔╝    ██║   ██║   ██║█████╔╝ 
    ██║   ██║██╔═██╗    ██║   ██║   ██║██╔═██╗ 
    ██║   ██║██║  ██╗   ██║   ╚██████╔╝██║  ██╗
    ╚═╝   ╚═╝╚═╝  ╚═╝   ╚═╝    ╚═════╝ ╚═╝  ╚═╝
```

# Semantic Search Engine

**An information retrieval system built over a TikTok corpus**  
*Evaluating keyword vs. structured Kibana queries using Precision & Recall*

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Elasticsearch](https://img.shields.io/badge/Elasticsearch-005571?style=flat&logo=elasticsearch&logoColor=white)
![Kibana](https://img.shields.io/badge/Kibana-005571?style=flat&logo=kibana&logoColor=white)
![University](https://img.shields.io/badge/University_Project-Information_Retrieval-8B5CF6?style=flat)

</div>

---

## Overview

This project constructs a **Gold Standard query set** of 20 natural language questions about TikTok, then evaluates two retrieval strategies against a Wikipedia-sourced document corpus using Elasticsearch and Kibana.

Each query is tested in two forms — a simple keyword query and a structured Kibana query — with Precision and Recall measured at cutoffs of **n=5** and **n=10**.

---

## Pipeline

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  TikTok Corpus  │────▶│  Gold Standard  │────▶│ Elasticsearch / │
│ (Wikipedia docs)│     │   20 queries    │     │     Kibana      │
└─────────────────┘     └─────────────────┘     └────────┬────────┘
                                                          │
                                      ┌───────────────────┴───────────────────┐
                                      ▼                                       ▼
                             ┌─────────────────┐                   ┌─────────────────┐
                             │  Keyword Query  │                   │  Kibana Query   │
                             │   (baseline)    │                   │  (structured)   │
                             └────────┬────────┘                   └────────┬────────┘
                                      └───────────────┬─────────────────────┘
                                                      ▼
                                           ┌─────────────────┐
                                           │  P & R @ n=5,   │
                                           │      n=10       │
                                           └─────────────────┘
```

---

## Repo Structure

```
📁 tiktok-semantic-search/
├── 📄 gold_standard.json          # 20 queries with keyword, Kibana form,
│                                  # exact answers & relevant doc matches
├── 🐍 eqs_evaluate_query_set.py   # Evaluation script — outputs P & R
│                                  # at n=5 and n=10 for both query types
└── 📝 README.md
```

---

## Gold Standard Format

Each of the 20 entries in `gold_standard.json` follows this structure:

```json
{
  "number": 1,
  "original_query": "Who are the most famous people on TikTok",
  "keyword_query": "TikTok famous followers",
  "kibana_query": {
    "query": {
      "multi_match": {
        "query": "TikTok famous followers",
        "fields": [],
        "type": "best_fields"
      }
    }
  },
  "answer_type": "PERSON",
  "exact_answers": ["Khaby Lame", "Kim Dracula", "Lisa and Lena"],
  "matches": [
    {
      "docid": "60917617",
      "sentences": [
        "The most-followed individual on the platform is Khaby Lame, with over 161.8 million followers."
      ]
    }
  ]
}
```

---

## Query Topics

The 20 queries span six themes:

| Theme | Queries |
|---|---|
| 👤 Influencers & creators | Most famous accounts, male/female creators, comedic creators, beauty/skincare personalities |
| 🎵 Music | Viral songs, popular artists, rap artists, song release dates |
| 🔒 Privacy & safety | Countries raising concerns, organisations responding to data issues |
| 🚫 Censorship & bans | Government restrictions, notable censorship events |
| 📈 Platform growth | When TikTok rose to global popularity |
| 🤝 Partnerships | Organisations partnering with TikTok on mental health awareness |

---

## Running the Evaluation

```bash
python eqs_evaluate_query_set.py
```

This outputs Precision and Recall at n=5 and n=10 for both query strategies.

---

## Results

### Table 1 — Keyword query

| # | Query | P@5 | R@5 | P@10 | R@10 |
|---|---|:---:|:---:|:---:|:---:|
| 1 | Who are the most famous people on TikTok | 0.60 | 0.60 | 0.50 | 1.00 |
| 2 | When did TikTok rise in popularity | 0.40 | 0.30 | 0.30 | 0.50 |
| 3 | Which artists have the most popular TikTok songs | 0.80 | 0.80 | 0.50 | 1.00 |
| 4 | Which countries banned or restricted TikTok | 0.80 | 0.80 | 0.50 | 1.00 |
| 5 | Which organisations partnered with TikTok on mental health | 0.20 | 0.20 | 0.20 | 0.40 |
| 6 | Which rap artists have gone viral on TikTok | 1.00 | 1.00 | 0.05 | 1.00 |
| 7 | What countries raised privacy/safety concerns about TikTok | 0.40 | 0.40 | 0.40 | 0.80 |
| 8 | Names of female TikTok content creators | 0.60 | 0.60 | 0.30 | 0.60 |
| 9 | TikTok influencers who made lip sync content | 0.40 | 0.40 | 0.40 | 0.80 |
| 10 | Organisations that acknowledged TikTok's social media influence | 0.80 | 0.80 | 0.50 | 1.00 |
| 11 | When were popular TikTok songs released | 0.80 | 0.80 | 0.50 | 1.00 |
| 12 | Events marking TikTok censorship in some countries | 0.80 | 0.80 | 0.40 | 0.80 |
| 13 | TikTok comedic content creators | 0.40 | 0.40 | 0.30 | 0.60 |
| 14 | TikTok personalities known for makeup/skincare/beauty | 0.60 | 0.60 | 0.40 | 0.80 |
| 15 | When did TikTok gain significant global popularity | 0.40 | 0.40 | 0.20 | 0.40 |
| 16 | Organisations raising concern about TikTok safety/privacy | 0.60 | 0.60 | 0.50 | 1.00 |
| 17 | Popular male TikTok influencers | 0.00 | 0.00 | 0.10 | 0.20 |
| 18 | Where are famous TikTok influencers from | 0.60 | 0.60 | 0.40 | 0.80 |
| 19 | When did TikTok influencers gain a large following | 0.20 | 0.20 | 0.30 | 0.60 |
| 20 | When were famous TikTok influencers born | 0.40 | 0.40 | 0.40 | 0.80 |

### Table 2 — Kibana query (**bold** = improved over keyword baseline)

| # | Query | P@5 | R@5 | P@10 | R@10 |
|---|---|:---:|:---:|:---:|:---:|
| 1 | Who are the most famous people on TikTok | 0.60 | 0.60 | 0.50 | 1.00 |
| 2 | When did TikTok rise in popularity | 0.40 | 0.30 | 0.30 | 0.50 |
| 3 | Which countries banned or restricted TikTok | 0.80 | 0.80 | 0.50 | 1.00 |
| 4 | Which artists have the most popular songs on TikTok | 0.80 | 0.80 | 0.50 | 1.00 |
| **5** | **Which organizations partnered with TikTok on mental health** | **0.20** | **0.20** | **0.30** | **0.60** |
| **6** | **Which rappers have gone viral on TikTok** | **1.00** | **1.00** | **0.50** | **1.00** |
| 7 | What countries raised privacy/safety concerns about TikTok | 0.20 | 0.20 | 0.20 | 0.40 |
| 8 | Names of female TikTok content creators | 0.60 | 0.60 | 0.30 | 0.60 |
| 9 | TikTok influencers who made lip sync content | 0.40 | 0.40 | 0.40 | 0.80 |
| 10 | Organisations that acknowledged TikTok's social media influence | 0.80 | 0.80 | 0.50 | 1.00 |
| 11 | When were popular TikTok songs released | 0.80 | 0.80 | 0.50 | 1.00 |
| 12 | Events marking TikTok censorship in some countries | 0.80 | 0.80 | 0.40 | 0.80 |
| 13 | TikTok comedic content creators | 0.40 | 0.40 | 0.30 | 0.60 |
| 14 | TikTok personalities known for makeup/skincare/beauty | 0.60 | 0.60 | 0.40 | 0.80 |
| 15 | When did TikTok gain significant global popularity | 0.20 | 0.20 | 0.20 | 0.40 |
| 16 | Organisations raising concern about TikTok safety/privacy | 0.60 | 0.60 | 0.50 | 1.00 |
| 17 | Popular male TikTok influencers | 0.00 | 0.00 | 0.10 | 0.20 |
| 18 | Where are famous TikTok influencers from | 0.60 | 0.60 | 0.40 | 0.80 |
| 19 | When did TikTok influencers gain a large following | 0.20 | 0.20 | 0.30 | 0.60 |
| 20 | When were famous TikTok influencers born | 0.40 | 0.40 | 0.40 | 0.80 |

---

## Key Findings

> 📉 **Precision decreases as n increases** — retrieving more documents brings in less relevant results, as expected in standard IR evaluation.

> 📊 **Recall does not consistently improve at n=10** — for queries like Q6 and Q7, recall stays flat, meaning relevant documents aren't ranked highly enough to surface at larger cutoffs.

> ✅ **Kibana structured queries outperform keyword queries on Q6** — phrase matching lifted P@10 from `0.05` → `0.50`, the single biggest improvement in the set.

> ❌ **Q17 fails both strategies** — "popular male TikTok influencers" returns P=0.00 at n=5 for both query types, revealing a vocabulary mismatch between the queries and how the corpus describes male creators.

---

## Technologies

- [Elasticsearch](https://www.elastic.co/elasticsearch/) — document indexing and retrieval
- [Kibana](https://www.elastic.co/kibana/) — structured query construction and execution
- [Python](https://www.python.org/) — evaluation script
- JSON — gold standard format

---

<div align="center">
<sub>Built as part of a university Information Retrieval module</sub>
</div>
