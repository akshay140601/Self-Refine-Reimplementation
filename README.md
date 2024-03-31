# Self-Refine-Reimplementation
CMU Advanced NLP - Assignment 3

## Abstract
For the research project, we plan to work on building an NLP system capable of generating CAD models using natural language queries. Since CAD design is an iterative process, achieving the best CAD design in a single attempt is uncommon. In this assignment, our primary focus was on surveying language models specifically used for code generation problems and exploring methods to provide feedback to the model to improve upon its previous generations. After conducting a literature survey, we found that the paper "SELF-REFINE: Iterative Refinement with Self-Feedback" most closely resembles the feedback loop in our architecture. We have re-implemented the code optimization task of this paper.

## Setup
- Install the prompt-lib by running the following commands:
  ```
  git clone https://github.com/reasoning-machines/prompt-lib
  pip install prompt-lib/
   ```
- To run the PIE task:
  ```
  python -u src/pie/run.py --slow_programs_file data/tasks/pie/codenet-python-test-1k.jsonl --max_attempts 4 --outfile data/tasks/pie/output --feedback_type rich
  ```

  ## Instructions for evaluating runtime for PIE experiments
  From the self-refine outputs, create a flattened version of the outputs, and then use the src/pie/prep_for_pie_eval.py to get the flattened outputs.
  Then, download the test cases required for evaluating the codes. It can be found [here](https://drive.google.com/file/d/1migwX4wpED0gDDxn7gS6q55vWeXIDgId/view)

1. Step1 (Construct yaml file): For evaluating runtime for PIE experiments, we need a yaml file that contains information about the dataset, the model outputs, and the reference file. Note that self-refine generates outputs in a slightly different format. While Self-Refine generates the outputs in an array (one version per refinement step), the evaluation requires the program to be present in a single column as a script.  src/pie/prep_for_pie_eval.py creates a single file where the output from the i^th step present in the attempt_i_code column. This produces a json file which is 'output4_changing_output.jsonl' in our case. The following is the output for evaluating the initial output (y0).
```
  inputs_outputs_basepath: "data/sample/generated_test_cases/generated_test_cases/"
  reference_file_path: "data/tasks/pie/ref.jsonl"
  num_problems_to_evaluate: -1
  num_trials: 10
  ignore_first_k: 0
  max_time_per_run: 10
  temp_dir: null
  model_generated_potentially_faster_code_col: "attempt_0_code"
  slow_code_col: "input"
  reference_code_col: "target"
  is_prompt_based: false
  language: python
  return_if_acc_below: 1.0
  cpu_number: 7
  output_report_file_path: "report/chatgpt/output.pie.jsonl.0.report"
  model_generated_outputs_path: "report/chatgpt/output4_changing_output.jsonl"
```
2. Step 2: After constructing the yaml file, run the following command to generate the reports.
    ```
    python src/pie/run_eval.py --eval_config report/chatgpt/config.yaml.0
    ```
    config.yaml.0 is the yaml file constructed above. If you run self-refine for 4 time-steps, you should create 4 yaml files and run this evaluation 4 times.
3. Step 3: Run the src/pie/pie_eval.py to generate results (number of codes optimized, speedup wrt input). Make sure to download 'R' software.
