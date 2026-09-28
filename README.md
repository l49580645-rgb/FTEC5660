# FTEC5660 Homework 1: Receipt Chain

Build a LangChain pipeline that reads every supermarket receipt in a folder
with the vision-capable DeepSeek Flash model and answers these two questions:

1. How much money did I spend in total for these bills?
2. How much would I have had to pay without the discount?

For this homework, **amount spent** means the final payment after the receipt's
rounding line. **Without the discount** means the sum of the original positive
item prices: add back every promotion, coupon, member, app, packaging-damage,
and percentage discount, but do not add back rounding.

## Student task

Only edit the two functions in `hw1.py` that contain `### YOUR CODE HERE`:

- `build_chain()` creates your LangChain chain.
- `answer_queries()` runs the chain on the receipt images and returns one final
  response for each question.

You may use prompt chaining, routing, parallel calls, reflection, or a
combination. Your final responses should each contain one HKD amount. Do not
hard-code filenames or public answers; grading uses unseen receipt folders.

## Setup and public test

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Put your DeepSeek key after `DEEPSEEK_API_KEY=` in `.env`, then run:

```bash
python3 hw1.py --image-folder public_test
```

The program creates `results.csv` in the current directory. Its columns are
`query`, `model_response`, and `correctness`. The public answers are in
`public_test/ground_truth.json`. The starter intentionally returns the dummy
response `please design your chain to answer these two queries.` so it runs
before you add any API code.

The required model is `deepseek-v4-flash-vision-exp`, the vision-capable
DeepSeek Flash model. JPEG, PNG, GIF, and WebP inputs are accepted by the
homework runner.


## Homework 1 solution: 
> to students: please fill your solution description here.

```mermaid
flowchart TD
    A([Start]) --> B[/Receipt Image(s)/]

    B --> C[Create Prompt Template<br/><br/>Define receipt analysis instructions<br/>Specify FINAL_PAYMENT and NO_DISCOUNT<br/>Set discount and rounding rules<br/>Require structured output]

    C --> D[Initialize DeepSeek Vision Model<br/><br/>Model: deepseek-v4-flash-vision-exp<br/>Temperature: 0]

    D --> E[Build LangChain Chain<br/><br/>Prompt | DeepSeek Vision Model]

    E --> F[Prepare Image Inputs<br/><br/>Convert local images to Data URLs<br/>Insert image URLs into prompt]

    F --> G[Batch Receipt Analysis<br/><br/>chain.batch(inputs)<br/>DeepSeek analyzes each receipt]

    G --> H[Extract and Validate Results<br/><br/>Parse FINAL_PAYMENT and NO_DISCOUNT<br/>using regular expressions]

    H --> I{Both values found?}

    I -->|No| J[Raise Error]
    I -->|Yes| K[Calculate Totals<br/><br/>Convert values to Decimal<br/>Accumulate all receipt amounts]

    K --> L[/Final Output<br/><br/>QUERY_1: Total Actual Payment<br/>QUERY_2: Total Amount Without Discounts/]

    L --> M([End])
```
