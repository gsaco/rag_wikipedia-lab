# Reflection: Multi-agent vs RAG

Multi-agent systems use several cooperating agents to break down a task, which helps handle ambiguity by distributing subquestions but may propagate contradictions across agents when they synthesize their answers. In contrast, RAG relies on concrete retrieved evidence and a single summarizer which encourages factuality but is sensitive to retrieval coverage.

Multi-agent pros: good for open-ended creative workflows; handles diverse perspectives; can propose multiple strategies.
Multi-agent cons: needs orchestration and stronger validation to avoid contradiction.

## Reflection: Multi-agent vs RAG

This project compares two broad approaches to complex tasks: multi-agent orchestration versus Retrieval-Augmented Generation (RAG). Both approaches have strengths and trade-offs; below is a short, practical comparison with examples and evaluation considerations.

1) How each handles ambiguity and contradictions
- Multi-agent: Breaks complex tasks into smaller subproblems handled by specialized agents (e.g., retriever, validator, summarizer). This enables diversity of perspectives and parallel exploration of ambiguous questions. However, because agents typically operate separately then synthesize results, contradictions or inconsistent assumptions can arise; they must be resolved either automatically (through additional agents) or manually.
- RAG: Uses a single generative model grounded in retrieved documents. Ambiguity is handled by returning evidence with the LLM's output and letting the model prioritize the most relevant facts. RAG reduces ungrounded hallucinations but depends on retrieval coverage — if key evidence is missing, the model may still create plausible but incorrect text.

2) How each handles factuality and verification
- Multi-agent: Agents can be built to perform verification (fact-checkers) and cross-check outputs between agents. This can increase factuality but requires careful design; added verification increases pipeline complexity and compute.
- RAG: Favors factuality through grounding; generated content can be traced back to retrieved chunks. Quality depends on embeddings, retrieval similarity metrics, and the corpus scope. Including provenance metadata (document IDs/retrieval snippets) is useful for auditing and grading.

3) Retrieval coverage and limitations
- Multi-agent: Can combine retrieval at multiple levels (different retrievers or knowledge sources) and then reconcile. This helps when data sources are heterogeneous but again increases orchestration complexity.
- RAG: Simpler setup — embed + vector store + retriever — but if the vector store lacks coverage for a topic, no single LLM prompt can recover missing facts. RAG is ideal when a curated corpus contains the necessary knowledge.

4) When to prefer which approach
- Prefer multi-agent workflows when you need multi-step reasoning across diverse data types, or when you want a system that explicitly cross-validates multiple answers (for open-ended ideation, design, or brainstorm). Multi-agent wins on creativity and modularity.
- Prefer RAG when factual fidelity, auditability, and interpretability are essential (e.g., clinical, legal, and policy contexts). RAG is simpler to implement and easier to validate, especially for grading or compliance.

5) Concrete suggestions for improving either approach
- For multi-agent: add a small verifier agent that rejects or flags contradictions. Use overlap retrieval strategies and prompt templates to harmonize assumptions across agents.
- For RAG: expand corpus breadth (more pages, domain-specific docs), add entity-level retrieval, and implement retrieval-augmented prompts that explicitly cite source IDs. Periodically re-run retrieval with different k (top-k) to test stability.

6) Evaluation and metrics
- Precision of factual claims: measure how many factual statements are grounded in retrieved documents.
- Coverage: how often the system locates relevant documents for queries.
- Hallucination rate: frequency of unverified or incorrect statements.
- Human evaluation: graders rate coherence, readability, and faithfulness on a Likert scale.

Conclusion: Both approaches complement each other. Multi-agent designs excel at open creative tasks, while RAG provides a compact, auditable pipeline more suitable for factual tasks — which is why RAG was the fit for this homework focused on evidence-grounded summarization.
