# Self-Refine-Reimplementation
CMU Advanced NLP - Assignment 3

## Abstract
In this assignment, our primary focus was on surveying language models specifically used for code generation problems and exploring methods to provide feedback to the model to improve upon its previous generations. After conducting a literature survey, we found that the paper "SELF-REFINE: Iterative Refinement with Self-Feedback" is the closest to what we are trying to implement in Assignment 4. We have re-implemented the code optimization part in this paper.

## Setup
- Install the '''prompt-lib''' by running the following commands:
  ```
  git clone https://github.com/reasoning-machines/prompt-lib
  pip install prompt-lib/
   ```
- To run the PIE task:
  ```
  python -u src/pie/run.py --slow_programs_file data/tasks/pie/codenet-python-test-1k.jsonl --max_attempts 4 --outfile data/tasks/pie/output --feedback_type rich
  ```

  ## Instructions for evaluating runtime for PIE experiments
  TLDR: From the self-refine outputs, create a flattened version of the outputs, and then use the PIE repo to evaluate the runtime and get a report. Parse the report using '''src/pie/pie_eval.py.'''
  
