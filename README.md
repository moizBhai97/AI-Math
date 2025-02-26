1. For each problem, copy the input $M$ times to define the initial batch of prompts to provide the model. These effectively define the number of candidates one uses for self-consistency / majority voting.
2. Sample $M$ completions until a complete block of Python code is produced (like the DeepSeekMath Instruct/RL models, our model produces code blocks in the ToRA format).
3. Execute each Python block and concatenate the output, including tracebacks if they appear.
4. Repeat $N$ times to produce a set of reasoning traces of width $M$ and depth $N$. If a trace fails to produce sensible outputs (e.g. incomplete code blocks or no `\boxed{}` output) prune that trace.
5. Postprocess the solution candidates and then apply majority voting to select the final answer

To accelerate inference we used [vLLM](https://github.com/vllm-project/vllm) and 8-bit models that were quantized with [AutoGPTQ](https://github.com/AutoGPTQ/AutoGPTQ). On modern hardware, one can skip the quantization step and run inference in standard 16-bit precision.