# CS5782-Final-Project-TreeOfThoughts

Tree of Thoughts Reimplementation
1. Introduction:
This repository contains our CS 4782/5782 final project, which attempts to reimplement key results from Tree of Thoughts: Deliberate Problem Solving with Large Language Models by Yao et al.
The paper’s main contribution is Tree of Thoughts (ToT), an inference-time search framework that lets LLMs generate, evaluate, and search over multiple intermediate reasoning paths instead of committing to one left-to-right chain.
2. Chosen Result:
We aimed to reproduce the paper’s comparison between Input-Output prompting (IO), Chain-of-Thought prompting (CoT), and Tree of Thoughts prompting (ToT) on Game of 24 and Creative Writing.
The key result we targeted was the paper’s finding that ToT substantially outperforms IO and CoT, especially on Game of 24, where the paper reports 74% success for ToT with breadth b=5 compared to 7.3% for IO and 4.0% for CoT.
3. GitHub Contents:
The repository contains task implementations, prompt files, experiment scripts, logs, result notebooks, and project documents.
Main folders/files include `tot/` for task and model code, `scripts/` for experiment commands, `logs/` for saved outputs, `results_analysis.ipynb` for result analysis, and the final report/poster PDFs.
4. Re-implementation Details:
We used Qwen3.6-28B through `llama-cpp-python` as the LLM for generation and evaluation, with no task-specific fine-tuning.
We tested IO, CoT, and ToT on Game of 24, Creative Writing, and an added 4x4 Sudoku task using the original ToT datasets where applicable and the Black-Phoenix 4x4 Sudoku dataset for the extension.
For Game of 24 and Creative Writing, we used BFS-style ToT search; for Sudoku, we implemented DFS with backtracking and deterministic rule checking.
Major modifications included rewriting prompts for Qwen compatibility and adding post-processing to remove `<think>` blocks, extra reasoning text, and formatting artifacts that interfered with parsing and scoring.
5. Reproduction Steps:
Install dependencies:
```bash
pip install -r requirements.txt
```
Run Game of 24 experiments:
```bash
python run.py --task game24 --task_start_index 0 --task_end_index 100 --naive_run --prompt_sample standard
python run.py --task game24 --task_start_index 0 --task_end_index 100 --naive_run --prompt_sample cot
python run.py --task game24 --task_start_index 0 --task_end_index 100 --method_generate propose --method_evaluate value --method_select greedy --n_generate_sample 1 --n_evaluate_sample 3 --n_select_sample 5
```
Run Creative Writing ToT using the paper-style settings:
```bash
python run.py --task text --task_start_index 0 --task_end_index 50 --method_generate sample --method_evaluate vote --method_select greedy --n_generate_sample 5 --n_evaluate_sample 5 --n_select_sample 1 --prompt_sample cot --temperature 1.0
```
Recommended compute: a CUDA-enabled GPU with enough VRAM for a quantized Qwen3.6-28B GGUF model. Our runs used an A100-class GPU environment.


6. Results / Insights:
Game of 24
Method	Ours	Paper
IO	6.8%	7.3%
CoT	3.1%	4.0%
ToT, b=5	67%	74%
Creative Writing
Method	Ours	Paper
IO	1.58	6.19
CoT	3.04	6.93
ToT, b=5	4.04	7.56
4x4 Sudoku Extension
Method	Success %
IO	9.2%
CoT	12%
ToT	85%
Our results support the paper’s main trend: ToT improves performance by searching over multiple candidate reasoning paths. The closest reproduction was Game of 24, while Creative Writing was more sensitive to prompt formatting and evaluator reliability.

7. Conclusion:
Tree of Thoughts was most effective on tasks with clear intermediate states, such as Game of 24 and Sudoku.
Our reimplementation showed that ToT can improve reasoning without fine-tuning, but also highlighted practical challenges in prompt transfer, output parsing, model choice, and subjective evaluation.


8. References:
Yao, S., Yu, D., Zhao, J., Shafran, I., Griffiths, T. L., Cao, Y., & Narasimhan, K. (2023). Tree of Thoughts: Deliberate Problem Solving with Large Language Models. arXiv:2305.10601.
Princeton NLP. Tree of Thoughts. https://github.com/princeton-nlp/tree-of-thought-llm
Black-Phoenix. 4x4 Sudoku Dataset. https://github.com/Black-Phoenix/4x4-Sudoku-Dataset


9. Acknowledgements
This project was completed as part of CS 4782/5782 at Cornell University.
We acknowledge the authors of the Tree of Thoughts paper and the Princeton NLP ToT repository, which provided the original framework and task setup that our project builds on.
