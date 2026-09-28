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

    A["1. Define the Receipt Analysis Task<br/><br/>
    Create a system prompt that tells the AI to analyze one supermarket receipt.<br/>
    The AI must extract FINAL_PAYMENT and NO_DISCOUNT.<br/>
    It is also given explicit rules for handling discounts, promotions, and rounding."]
    
    B["2. Create the Multimodal Prompt<br/><br/>
    Build a ChatPromptTemplate containing a system message and a human message.<br/>
    The human message includes the instruction to analyze the receipt<br/>
    and an image_url placeholder for the receipt image."]
    
    C["3. Initialize the DeepSeek Vision Model<br/><br/>
    Create ChatDeepSeek using the vision-capable model<br/>
    deepseek-v4-flash-vision-exp.<br/>
    Set temperature to 0 so the model produces stable and consistent results."]
    
    D["4. Build the LangChain Pipeline<br/><br/>
    Connect the prompt and the DeepSeek model using Prompt | Model.<br/>
    This creates a reusable LangChain chain that receives a receipt image<br/>
    and returns the AI's structured analysis."]
    
    E["5. Prepare the Receipt Images<br/><br/>
    Receive a list of local receipt image files.<br/>
    Convert each image into a Data URL using image_data_url().<br/>
    Store each converted image as an input object with an image_url field."]
    
    F["6. Analyze All Receipts with the AI<br/><br/>
    Send all prepared image inputs to the chain using chain.batch().<br/>
    DeepSeek analyzes each receipt independently and returns:<br/>
    FINAL_PAYMENT=xxx.xx<br/>
    NO_DISCOUNT=xxx.xx"]
    
    G["7. Extract and Aggregate the Amounts<br/><br/>
    Extract FINAL_PAYMENT and NO_DISCOUNT from each AI response using regular expressions.<br/>
    Convert the extracted strings into Decimal values for accurate monetary calculations.<br/>
    Add the values from all receipts to total_spent and total_no_discount."]
    
    H["8. Format and Return the Final Results<br/><br/>
    Format both totals to two decimal places.<br/>
    Add the HK$ currency symbol.<br/>
    Return QUERY_1 as the total actual payment and QUERY_2 as the total amount without discounts."]

    A --> B --> C --> D --> E --> F --> G --> H
```
