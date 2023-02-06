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
The fine-tuned GPT2 model would then 

