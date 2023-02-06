# gpt2-question-generation

This work was for my MSc project and the full document can be read here:  
https://drive.google.com/file/d/1an1wqCsra0kzw1loVm_HrHdkWHnIYygf

In short, GPT2 can be fine-tuned to generate questions when given structured prompts consisting of tags, context, and an answer selected from the context.  
For example, one training example could be:  
  [Context]: YouTube has cited the effectiveness of Content ID as one of the reasons why the site's rules were modified in December 2010 to allow some users to upload videos of unlimited length.  
  [Answer]: were modified  
  [Question]: What happened to the sites rules in Dec. 2010?  
  <|endoftext|>  

A generation example prompt could be:  
  [Context]: In the 1960 election to choose his successor, Eisenhower endorsed his own Vice President, Republican Richard Nixon against Democrat John F. Kennedy.  
  [Answer]: Richard Nixon  
  [Question]:   
The fine-tuned GPT2 model would then suggest a set of answers, which were then ranked by an LSTM by likelihood. (Ground truth was "Who did Eisenhower endorse for president in 1960?")  

A series of Google Colab notebooks illustrate how this was done.   
First the SQuAD question answering database was processed:  
https://colab.research.google.com/drive/1gcdbLZgnVZ0LWF26DHG7c65UIU4PrmZX?usp=sharing  

Next, coreference was needed to fit all necessary information into a single sentence. For example, in "John and Jill went to the shops. He picked up a baguette." it is not immediately clear to a machine who, or even what, "he" is. This needs to be resolved, and this used Spacy and Coreferee:  
https://colab.research.google.com/drive/1pw6_V0d6dSZRZCFkhl-EIAtPV13vjSog?usp=sharing  

Further post processing was needed on the SQuAD dataset to select the relevant sentence from the extract:  
https://colab.research.google.com/drive/1dcB96uPqSHe4JkkqNB3O06FSGCY20tgU?usp=sharing  

The most common question starts were found and the dataset reduced to them, to help limit the focus of the fine tuning:  
https://colab.research.google.com/drive/1XwYXt2zWrqDkokhJqW23PqrOVqebOaXQ?usp=sharing  

To generate questions, an answer needed to be selected, which was always a section from the context. This was done in the same way as above, but with only context and answer to train on:  
https://colab.research.google.com/drive/1iwAImnv2okV0rdW7nmJXBpw9gZXxwRr-?usp=sharing  

Generation of the answers:  
https://colab.research.google.com/drive/122uBYZaf2xuKGBZd1IHiMRHz0Mh-KL_S?usp=sharing  

The answers selected showed a high degree of similarity in counts of parts of speech and this was verified here:  
https://colab.research.google.com/drive/1qfmQhzIUdYla-Rs-aK3SG_xh8C8Gq9QL?usp=sharing  

GPT2 was then fine-tuned on a training subset of the SQuAD data in the above structured prompt format:  
https://colab.research.google.com/drive/1Z1C6S0wK0oBSadB3pPHvwO7CxGXTRFGS?usp=sharing  

The fine-tuned GPT2 model was used to generate possible questions:  
https://colab.research.google.com/drive/1Z1C6S0wK0oBSadB3pPHvwO7CxGXTRFGS?usp=sharing  

An LSTM was trained on questions in the SQuAD dataset, converting each word into parts of speech tags, then symbols:  
https://colab.research.google.com/drive/14MXHUXupPN-930gDzJtzBxvrMtbCRe1_?usp=sharing  

This model was then used to generate likelihoods for each part of speech tag in the generated sentences. The geometric mean of these was combined and some high scoring generate examples are below:  
What is the main language of Armenia?  
What is another name for the President JK Bridge?  
Who is the leader of the minority party?  
What is the primary flag-carrier in Portugal?  
Along with Eisenhower, Macmillan and de Gaulle, what leader attended the Four Powers Paris Summit?  

The results and data files from each can be used in a summary notebook here:  
https://colab.research.google.com/drive/1_aHYIamKz7HS2q-jUnGQvKQueYCe0IxO?usp=sharing  
The selected input example was:  
  'He then extended the theory to gravitational fields; he published a paper on general relativity in 1916, introducing his theory of gravitation.'  
With coreference resolution:  
  'Einstein then extended the theory to gravitational fields; Einstein published a paper on general relativity in 1916, introducing Einstein theory of gravitation'  
  
Generated questions and answers included:  
    'In what year did Einstein release his theory of gravitational fields? 1916'  
  'Who was responsible for the development of the theory of gravitational fields? Einstein'  
  'When did Einstein publish his theory of gravitational fields? 1916'  
