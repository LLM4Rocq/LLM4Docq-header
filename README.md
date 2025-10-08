# LLM4Docq-header
The goal of this repository is to work collaboratively on MathComp documentation using the [LLM4Docq](https://github.com/LLM4Rocq/LLM4Docq) pipeline.
More precisely, LLM4Docq relies on leveraging external LLMs, and to maximize performance, it requires careful prompting.

To avoid overflowing the LLM’s context and to improve results, we chunk the source code into contiguous pieces and ask the LLM to annotate them, given a context header that helps it understand each piece of code.
Here, a context header is not part of the MathComp source code, it is an external description used in the prompting process to give the LLM the necessary background for understanding the file.
In short, the LLM takes as input some general instructions (see the [prompt_template](prompt_template.txt)), a **context header**, and (in the next version thanks to this repository) some carefully **handcrafted examples** of well-chosen docstrings.

The main task is to **audit these context headers**, which were first generated synthetically using state-of-the-art reasoning models, and to add **precise and context-aware examples** to the prompt.
More simply, we consider three tasks for each source file:

- Audit an header
- Select 5 to 10 elements for annotation
- Manually add docstrings to previously selected elements

We are working with [this fork](https://github.com/theostos/math-comp/blob/llm4docq/) of MathComp.

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

Try to follow these guidelines:
1. **Use natural language only.** If possible, do not use any mathematical symbols (like <, ≤, =, etc.).  
2. **Write complete sentences.** Each docstring must be at least one clear, grammatically correct sentence.  
3. **Be explicit.** Spell out operations and relationships in plain English (e.g., "less than" instead of "<").  
4. **Be self-contained.** The reader should understand the meaning of the declaration without seeing the code.  
5. **Be embedding-friendly.** Do not use pronouns or external references like "this lemma" or "the following." Explicitly mention inputs and outputs.  
6. **Use high-level terms** like natural number, matrix, or polynomial.
7. **Be consistent.** If the element is a lemma, start the docstring with "This lemma states ..."; apply the same rule to all other kinds of elements.  
8. **Avoid referring to variable names when possible.** For example, instead of writing "Defines the minimal polynomial of a matrix A as the monic polynomial of least degree that evaluates to the zero matrix when A is substituted for the variable,..." prefer "Defines the minimal polynomial of a matrix as the monic polynomial of least degree that evaluates to the zero matrix when substituted for the variable."
