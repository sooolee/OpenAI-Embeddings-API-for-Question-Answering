# Question-Answering Model Using OpenAI Embeddings
In this project, I built a model that "generates" answers to questions given a context, such as a pdf document. 

I used OpenAI's Embeddings API along with their embedding model and completion model. 

Compared to finetuning LLMs on question-answering (such as https://huggingface.co/sooolee/roberta-base-finetuned-squad-v1), this approach allows building the model fast without finetuning, and also the model can take a much larger context data. 

* "text-embedding-ada-002" is the embedding model that converts each DataFrame row into an embedding. 
* "text-davinci-003" is the completion model that will generate answers based on the the best matches (cosign similarities) between a question and context. 

## Demo
🚧 The demo is on the way! 🚧

## Model Development Process
1. Convert a pdf file into a DataFrame that the embedding model can process. Break down the entire document into sub-texts, where a good chunk of overlapping between neighboring sub-texts is captured in order not to lose the infomation around the breaking point. 
- Note: The embedding model can take up to 8192 tokens in length per single request. This means each sub-text can take up to this size. My model has much smaller size of sub-texts. 
2. Compute embedding for each row of the DataFrame using the embedding model, returning a dictionary that maps between each embedding vector and the index of the row that it corresponds to. 
3. Compare similarities between document and query. Calculate the cosine similarities between two vectors (embeddings of a query and each row (of the doc DataFrame)). Sort the scores in order of the highest similarities. 
4. Construct the prompt with sections with highest similarities: The completion model needs a specific instruction for each question along with the context. Instruction also includes the instruction in case of no match. Context means the sub-texts with highest similarities.
5. Use the completion model to get the answer! The model takes a context (a document in pdf), a query (your question), and the embeddings dictionary (generated from Step 2 above).

