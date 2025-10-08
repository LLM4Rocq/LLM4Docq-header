# LLM4Docq-header
The goal of this repository is to work collaboratively on MathComp documentation using the [LLM4Docq](https://github.com/LLM4Rocq/LLM4Docq) pipeline.
More precisely, LLM4Docq relies on leveraging external LLMs, and to maximize performance, it requires careful prompting.

To avoid overflowing the LLM’s context and to improve results, we chunk the source code into contiguous pieces and ask the LLM to annotate them, given a context header that helps it understand each piece of code.
In short, the LLM takes as input some general instructions (see the [prompt_template](prompt_template.txt)), a **context header**, and (in the next version thanks to this repository) some carefully **handcrafted examples** of well-chosen docstrings.

The main task is to **audit these context headers**, which were first generated synthetically using state-of-the-art reasoning models, and to add **precise and context-aware examples** to the prompt.
More simply, we consider three tasks for each source file:

- Audit an header
- Select 5 to 10 elements for annotation
- Manually add docstrings to previously selected elements

We are working with [this fork](https://github.com/theostos/math-comp/blob/master/) of MathComp.

**To balance the work, please indicate on the following [link](https://notes.inria.fr/dLNK-EW_S8SJdvc0u2YHqA#) what you are working on, be careful not to duplicate someone else's work**

### Audit a header

Headers to be amended are located in the `headers` folder of the repository.
Contributors should edit these files directly to correct, clarify, or enrich the generated context headers.
The goal is to make them more accurate, coherent, and informative for the LLM.
The guiding question is:

"Given this header, could I understand almost any chunk (~100 lines) of the associated source file without more information?"

### Select 5 to 10 elements for annotation

For each source file, identify 5 to 10 relevant elements (functions, lemmas, definitions, etc.) that are representative of the challenges presented in the current file.
They should be as diverse as possible to cover different kinds of constructs and difficulties.

Selections and annotations are stored in the `examples/` folder (placeholder templates are already provided there), following the JSON format described below.

### Manually add docstrings

Each example should follow this structure:

```json
[
    {
        "name": "element_name",
        "line": 42,
        "docstring": "A short, clear explanation of what this element does."
    }
]
```

