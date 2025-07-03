# General Description of the Noteboook:

The notebook is a powerful resume parser capable of extracting information from variuos files. It leverages a combination of document loading tools from the LangChain library and a pre-trained Large Language Model (LLM) from Hugging Face's Transformers library.

## The process invovles:

* A extract_text_from_document function that reads the contents e=mentioned in the file. so far the only supported files are .pdf, .docs, .docx.
* The extratced text is then fed into ChatPromptTemplate. This prompt acts as an instruction set for the LLM, detailing the specific task of resume parsing and providing a strict JSON schema that the LLM must adhere to for its output.
* A LLama 3 varient is loaded and configured using huggingface. The LLM processes the resume text and the prompt, generating a response that is expected to be a JSON string.
* Finally, the LLMs string output is parsed into a structured JSON object and saved directly to a file, which only contains the resumes key details.

# Justification for the Chosen Method:

I selected this particular method, primarily due to the constraint of having only one sample resume for development.

## The limitations that I had to address:

* Training a large Transformer model from scratch requires massive amounts of diverse data. With just a single or more sample resumes, attempting to train a Transformer would lead to severe overfitting, where the model would simply memorize the one resume and perform terribly on any other. It would lack the ability to generalize to different resume layouts, phrasing, or content.
* While pre-trained Transformers, have a vast general understanding of language, they aren't inherently optimized for the specified task at hand. I found out that directly using them without any specific instruction or prompt engineering the results were not accurate enough to pull off the precise extraction needed for the particualar JSON schema asked.
* My initial thought might have been to use a rule-based approach involving regular expressions (regex). This approach is deterministic and doesn't require complex models. However, if different resumes are used, regex becomes more and more complicated. They avoided the need for transformers at all.

## Advantages of using this particular method:

* The LLM already possesses a vast understanding of human language, common entities and other information from its pretraining on diverse datasets.
* By providing a clear and precise prompt, including the exact JSON schema, I can guide the LLM to perform the specific information extraction task.
* Using LangChain's document loaders allows the same core LLM parsing logic to be applied across PDF, DOCX, and DOC files
* Unlike rigid regex rules, the LLM can understand the meaning and context of information, even if the resume's formatting or phrasing is slightly different from what it has seen before.
