<div align="center">
  <img src="logo_with_name_white.png" alt="Logo" width="400"/>
</div>

# Academic Paper Search Benchmarks

A comprehensive respository dedicated to evaluation of academic paper search engines that provides test search queries and evaluation metrics.

The test search queries are distributed across many categories to enable fine-grained evaluations (see below).

<!---
fields, query type, query lengths, problem framing, specificity level and reseach stage (see below for details).-->

## 📊 Test Sets
### List of Test Sets
1. [`test_sets/computer_science_ai_search_queries.json`](https://github.com/paperlantern-ai/academic_paper_search_benchmarks/blob/main/test_sets/computer_science_ai_search_queries.json)

    - 200 search queries
    - covering AI, machine learning, and related computer science domains.  
    - including research areas like neural networks, reinforcement learning, computer vision, and natural language processing.

2. [`test_sets/computer_science_non_ai_search_queries.json`](https://github.com/paperlantern-ai/academic_paper_search_benchmarks/blob/main/test_sets/computer_science_non_ai_search_queries.json)
    - 200 search queries
    - covering all non-AI computer science domains
    - including research areas like algorithms, systems, theory, security, databases, cryptography and software engineering


### 🔍 Search Query Dimensions

The test sets systematically covers multiple dimensions of academic search:

<table>
<tr style="background-color: #000000; color: white;">
<th>Dimension</th>
<th>Value</th>
</tr>
<tr style="background-color: #e3f2fd;">
<td rowspan="5" style="text-align: center; vertical-align: middle; background-color: #e3f2fd;"><strong>Query Types</strong></td>
<td>Natural Language Queries</td>
</tr>
<tr style="background-color: #e3f2fd;">
<td>Specific methodologies/techniques</td>
</tr>
<tr style="background-color: #e3f2fd;">
<td>Major concepts/theories</td>
</tr>
<tr style="background-color: #e3f2fd;">
<td>How To</td>
</tr>
<tr style="background-color: #e3f2fd;">
<td>Highly technical terminology</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td rowspan="3" style="text-align: center; vertical-align: middle; background-color: #f5f5f5;"><strong>Query Length</strong></td>
<td>Few words</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td>Sentence</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td>Multi-sentence</td>
</tr>
<tr style="background-color: #e3f2fd;">
<td rowspan="3" style="text-align: center; vertical-align: middle; background-color: #e3f2fd;"><strong>Specificity Levels</strong></td>
<td>Very specific</td>
</tr>
<tr style="background-color: #e3f2fd;">
<td>Focused</td>
</tr>
<tr style="background-color: #e3f2fd;">
<td>Broad</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td rowspan="5" style="text-align: center; vertical-align: middle; background-color: #f5f5f5;"><strong>Research Stages</strong></td>
<td>Starting research</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td>Literature review</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td>Implementation focused</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td>Seeking comparisons</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td>Looking for gaps</td>
</tr>
<tr style="background-color: #e3f2fd;">
<td rowspan="4" style="text-align: center; vertical-align: middle; background-color: #e3f2fd;"><strong>Problem Framing</strong></td>
<td>Problem to solve</td>
</tr>
<tr style="background-color: #e3f2fd;">
<td>Technique to learn</td>
</tr>
<tr style="background-color: #e3f2fd;">
<td>Comparison to make</td>
</tr>
<tr style="background-color: #e3f2fd;">
<td>Gap to identify</td>
</tr>
</table>

### 📋 Test Set Structure (JSON Format)
Each test set is a JSON object with query keys - each pointing to a search query and metadata:
```json
{
  "query_0": {
    "settings": {
      "query_type": "Niche Areas",
      "length": "Multi-sentence",
      "problem_framing": "Technique to learn",
      "specificity_level": "Focused",
      "research_stage": "Seeking comparisons"
    },
    "search_query": "Compare different architectural frameworks for large language model agents, focusing on their respective strengths in complex task execution. What are the trade-offs between single-LLM and multi-agent systems for real-world problem-solving, particularly concerning efficiency and robustness?"
  },
  "query_1": { ... },
  ...
}
```

## 🤖 LLM-Based Evaluation Methodology
<div style="background: #e8f5e8; padding: 15px; border-left: 4px solid #4caf50; margin: 20px 0;">
  <strong>🧑‍🔬 Expert-Level Assessment:</strong> Our evaluation replicates how expert researchers actually judge paper relevance - understanding research intent, semantic relationships, and domain-specific context.
</div>

There is no existing approach for reliably evaluating the quality of academic paper search. Hence, we created an easy-to-use **Query-Paper Relevance Score** that combines:  
**🧠 Semantic Understanding** - Focuses on research intent and conceptual alignment, not keyword overlap  
**🔍 Relationship Directionality** - Distinguishes subject vs object roles ("A influences B" ≠ "B influences A")  
**📚 Domain Expertise** - Recognizes technical terms have domain-specific meanings in academic contexts  
**⚖️ Nuanced Distinctions** - 6-point rubric captures "directly addresses" vs "same area" vs "tangential"   
**🎯 Implicit Relationships** - Considers both explicit mentions and implicit connections

For a given search engine and search query, we compute the **Query-Paper Relevance Score** on each (query, paper title, paper abstract) triplet that the search engine generates. [This is done by prompting a LLM with a detailed system prompt and passing the (search_query, paper title, paper abstract) as the prompt](https://github.com/paperlantern-ai/academic_paper_search_benchmarks/blob/main/evaluation_prompts.py).

The system prompt defines a 0-to-5 Likert Scale with rubrics that capture subtle but critical distinctions between papers that directly address a query versus those in the same general area, enabling more precise differentiation of retrieval quality across diverse academic search scenarios. We multiply the returned relevance score by x20 to report a more intuitive 0-100 scale.

The system prompt also asks the LLM to produce a Confidence scoring (0-10) and a one sentence summary for it's reasoning. We found that asking for these inputs significantly improved the quality and consistency of the reported **Query-Paper Relevance Score**.

### Evaluation Scale (0-100)

| Score | Level | Description |
|:-------:|-------|-------------|
| **0** | No Relevance | No meaningful scholarly connection |
| **20** | Tangential Relevance | Minimal substantive connection |
| **40** | Peripheral Treatment | Secondary discussion of topic |
| **60** | Substantial Coverage | Significant component of paper's scope |
| **80** | Primary Topical Focus | Central theme aligns with query domain |
| **100** | Direct Correspondence | Paper directly addresses the research question |

### LLM Settings
```
llm: gemini-2.5-flash-lite-preview-06-17 # can change to other models
max_tokens: 4096
temperature: 0.01  # Critical: ensures valid JSON output
thinking_budget: 0  # Summary statement provides sufficient reasoning
```

### Example Output

```json
{
  "paper_query_relevance": {
    "relevanceScore": 80,
    "confidenceLevel": 9,
    "summaryStatement": "Paper's central focus on transformer attention mechanisms directly relates to query about long sequence implementation."
  }
}
```

## 🤝 Contributing

We welcome contributions to improve this research resource! Please:
- Submit issues for additional query types or academic domains
- Propose new metadata dimensions for richer evaluation
- Share evaluation results and insights using this dataset
- Suggest improvements to the LLM evaluation methodology

## 📄 Citation

```bibtex
@dataset{academic_paper_search_benchmarks_2025,
  title={Academic Paper Search Benchmarks 2025},
  author={Paper Lantern},
  year={2025},
  url={https://github.com/paperlantern-ai/academic_paper_search_benchmarks}
}
```

## 📜 License
Apache 2.0

---
**🏮 Paper Lantern AI**: [https://paperlantern.ai/](https://paperlantern.ai/)  
**🔗 Dataset**: [https://huggingface.co/paperlantern](https://huggingface.co/paperlantern)  
**📧 Contact**: contact@paperlantern.ai
