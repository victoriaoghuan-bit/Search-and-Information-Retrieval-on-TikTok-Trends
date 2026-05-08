# Search-and-Information-Retrieval-on-TikTok-Trends
Semantic search and information retrieval project using TikTok-related datasets, JSON queries, Kibana/Elasticsearch queries, and precision/recall evaluation.

Overview
This project constructs a Gold Standard query set for evaluating information retrieval performance on TikTok-related content. It tests two query strategies — keyword queries and structured Kibana queries — measuring Precision and Recall at cutoffs of n=5 and n=10.
The gold standard covers 20 natural language queries spanning topics such as TikTok influencers, viral music, privacy controversies, censorship events, and platform growth.

Gold Standard Format
The .json file contains 20 queries, each with:
FieldDescriptionoriginal_queryFull natural language questionkeyword_queryExtracted keywords for baseline retrievalkibana_queryStructured Elasticsearch query (multi-match, phrase search, must/should clauses)answer_typeExpected answer type (PERSON, DATE, LOCATION, etc.)exact_answersGround truth answer entitiesmatchesRelevant document IDs and supporting sentences

Example Query
json{
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
  "exact_answers": ["Kim Dracula", "Khaby Lame", "Lisa and Lena", "Davis Burleson", "Vanessa Lopes"]
}
Query Topics
The 20 queries cover:

Influencers — most famous accounts, male/female creators, comedic creators, beauty/skincare personalities
Music — viral songs, popular artists, rap artists on TikTok
Privacy & Safety — countries raising concerns, organisations responding to data issues
Censorship & Bans — government restrictions, notable censorship events
Platform Growth — when TikTok rose to global popularity
Partnerships — organisations partnering with TikTok on mental health awareness

Evaluation
Run the provided evaluation script to compute Precision and Recall for both query types:
bashpython eqs_evaluate_query_set.py
The script outputs P/R values at n=5 and n=10 for:

keyword_query — simple keyword-based retrieval
kibana_query — structured Elasticsearch query

Results Summary
Table 1 — Keyword Query Performance
#QueryP@5R@5P@10R@101Who are the most famous people on TikTok0.600.600.501.002When did TikTok rise in popularity0.400.300.300.503Which artists have the most popular TikTok songs0.800.800.501.004Which countries banned or restricted TikTok0.800.800.501.005Which organisations partnered with TikTok on mental health0.200.200.200.406Which rap artists have gone viral on TikTok1.001.000.051.007What countries raised privacy/safety concerns about TikTok0.400.400.400.808Names of female TikTok content creators0.600.600.300.609TikTok influencers who made lip sync content0.400.400.400.8010Organisations that acknowledged TikTok's social media influence0.800.800.501.0011When were popular TikTok songs released0.800.800.501.0012Events marking TikTok censorship in some countries0.800.800.400.8013TikTok comedic content creators0.400.400.300.6014TikTok personalities known for makeup/skincare/beauty0.600.600.400.8015When did TikTok gain significant global popularity0.400.400.200.4016Organisations raising concern about TikTok safety/privacy0.600.600.501.0017Popular male TikTok influencers0.000.000.100.2018Where are famous TikTok influencers from0.600.600.400.8019When did TikTok influencers gain a large following0.200.200.300.6020When were famous TikTok influencers born0.400.400.400.80
Key Findings

Precision decreases as n increases — retrieving more documents brings in less relevant results, as expected.
Recall does not consistently increase with n — for some queries (e.g. Q6, Q7) recall stays flat or drops, indicating that relevant documents are not ranked highly enough to appear at n=10.
Kibana structured queries outperform keyword queries on several queries (e.g. Q6 improves from P@10=0.05 to P@10=0.50) by enabling phrase matching and field-level constraints.
Hardest queries: Q17 (popular male influencers) — both keyword and Kibana queries return P=0.00 at n=5, suggesting the keyword set does not align well with how the corpus describes male influencers.

Technologies

Elasticsearch / Kibana — document indexing and query execution
Python — evaluation script
JSON — gold standard format

Course Context
Built as part of a university Information Retrieval module. The task involved:

Selecting a topic corpus (TikTok-related Wikipedia articles)
Designing 20 natural language queries with ground truth answers
Constructing keyword and structured Kibana queries
Evaluating retrieval quality using Precision and Recall at n=5 and n=10
