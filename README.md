# GPT-2 Context Buffering

This repo tests how the size of a prompt's context window affects GPT-2 Small's resistance to activation steering. 

We extracted a self-referential vector and injected it into the residual stream at Layer 5. We then ran a sweep using deterministic decoding (temp 0.0) to see how high the injection multiplier (alpha) needed to be before the model's output broke into a repetitive loop.

* **Short Prompt:** The steering vector overwhelmed the model at alpha = 4.
* **Long Prompt:** When the prompt was padded with extra text, the model absorbed the injection and didn't break until alpha = 13.

The results show that having more tokens in the KV cache dilutes the impact of the steering vector.

### Results
<img width="989" height="590" alt="image" src="https://github.com/user-attachments/assets/1563a614-03ef-453b-bedf-b39b6a0373f1" />


The residual stream data gets noisy right before the break at alpha=13, so it's unclear if this is a clean semantic transition or just basic memory failure. However, the buffering effect itself is clear.

### Files
* `code.ipynb`: The Jupyter notebook containing the TransformerLens hooks, the extraction script, and the test sweep.
