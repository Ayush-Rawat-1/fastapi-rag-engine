# 🚀 FastAPI RAG Engine

**Live endpoint:** `POST https://happybro-pdf-qa.hf.space/api/v1/hackrx/run`

---

## Flow in 4 sentences
1. **POST** a JSON body with a public PDF URL and a list of questions.  
2. FastAPI downloads the PDF, chunks it, embeds each chunk with `all-MiniLM-L12-v2`, and stores the vectors in FAISS.  
3. For every question, the most relevant chunks are retrieved and sent to `llama-3.1-70b-instruct` on Groq.  
4. You receive a JSON array of concise answers, one per question, in under 3 seconds.

---

## Quick test

```bash
curl -X POST https://happybro-pdf-qa.hf.space/api/v1/hackrx/run \
  -H "Content-Type: application/json" \
  -d '{
    "documents": "https://hackrx.blob.core.windows.net/assets/policy.pdf?sv=2023-01-03&st=2025-07-04T09%3A11%3A24Z&se=2027-07-05T09%3A11%3A00Z&sr=b&sp=r&sig=N4a9OU0w0QXO6AOIBiu4bpl7AXvEZogeT%2FjUHNO7HzQ%3D",
    "questions": [
        "What is the grace period for premium payment under the National Parivar Mediclaim Plus Policy?",
        "What is the waiting period for pre-existing diseases (PED) to be covered?]
      }'
```
## Response
```JSON
{
    "answers": [
        "The grace period for premium payment under the National Parivar Mediclaim Plus Policy is thirty days, during which coverage shall not be available if no premium is received, and payment made within this period will continue the policy without loss of continuity benefits.",
        "The waiting period for pre-existing diseases to be covered is 36 months of continuous coverage after the date of inception of the first policy. However, if the insured person has continuous coverage without a break as per IRDAI portability norms, the waiting period can be reduced to the extent of prior coverage. If the insured has prior coverage, the waiting period will be reduced accordingly."
        ]
}
```
| Field     | Type   | Example                                                |
| --------- | ------ | ------------------------------------------------------ |
| documents | string | `"https://example.com/paper.pdf"`                      |
| questions | list   | `["Who are the authors?", "What is the ROUGE score?"]` |

## Stack
FastAPI → LangChain → Sentence-Transformers → FAISS → Groq (llama-3.1-70b-instruct)
