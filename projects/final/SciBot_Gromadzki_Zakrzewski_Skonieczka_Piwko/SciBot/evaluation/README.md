# ChatBot evaluation

Evaluation was split into two parts - embedding evaluation and LLM evaluation.

## Embedding evaluation

We have tested 4 diffrent embedding models - ```snowflake-arctic-embed```, ```mxbai-embed-large```, ```nomic-embed-text``` and ```instructor-xl```

1. For each embedding model, we created a dedicated vector store containing all documents (`embeddings/create_vdbs.ipynb`).
2. Using the ```llama3.1:8b``` language model, we generated 100 modified versions of the original documents, which were stored in the vector stores (```embeddings/gen_mod_documents.ipynb```).
3. These modified documents served as query inputs for each vector store, with the goal of ranking the original documents as highly as possible.
4. We then evaluated performance by calculating metrics such as HR@4, MRR@4, nDCG, and MAP@4.(```embeddings/evaluate.ipynb```)

## LLM evaluation

We have tested 4 diffrent large language models - `qwen2.5:3b`, `qwen2.5:7b`, `llama3.2:3b` and `llama3.1:8b`.

Both `qwen2.5:7b` and `llama3.1:8b` models have been quantized to `Q4_0`, while `qwen2.5:3b` and `llama3.2:3b` are quantized to `Q4_K_M`.

1. We prepared a set of 25 questions, each with an answer found within the embedded documents().(`data/llm_eval/questions.txt`)
2. For each question, answers were generated by all models.(`llms/gen_real_outputs.ipynb`)
3. We created ground truth answers for all 25 questions using the OpenAI API.(`llms/gen_reference_chatgpt.ipynb`)
4. Using this data, we evaluated metrics such as Answer Relevancy, Faithfulness, Contextual Precision, Contextual Recall, Contextual Relevancy, and Hallucination with the `deepeval` Python package.(`llms/evaluate.ipynb`)

All results were saved as `csv` files to `data/results`.

### Note

Additional packages are needed for evaluation - ```pip install -r evaluation/requirements.txt```
