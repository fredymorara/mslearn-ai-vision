# Generative AI Vision Lab - Completion

## Overview
This document serves as a record of successfully completing the **gen-ai-vision** lab.

## What We Accomplished
1. **Configured Environment Variables**: 
   - Updated the `.env` file to correctly point to the Azure AI Services Inference Endpoint (`https://fredm-ai-103-labs-resource.services.ai.azure.com/openai/v1`).
   - Configured the model deployment name to `gpt-5.2`.
2. **Updated the Python Script**:
   - Completed `image-chat-app.py` by setting up the OpenAI client using `AzureDefaultCredential` for passwordless authentication.
   - Submitted a multi-modal prompt containing both a text instruction and an image URL (a photo of an orange).
3. **Troubleshooting**:
   - Successfully resolved `400 Bad Request` endpoint errors by identifying that the `.env` endpoint was mistakenly set to the project API endpoint instead of the standard `openai/v1` serverless endpoint.
4. **Validation**:
   - Verified that the model could successfully process the image and provide relevant answers.
   - **Successfully ran the prompt in the Azure Chat Playground**.
   - **Successfully ran the Python script locally via the terminal**.

## Execution Output
Running the script locally yielded the following successful response from the model when asked to suggest recipes for the image of an orange:

```text
Ask a question about the image
(or type 'quit' to exit)
Suggest some recipes that include this fruit
Getting a response ...

That’s an **orange (citrus)**. Here are some tasty recipe ideas that use oranges (juice + zest), with quick how-to notes:

## Sweet recipes
1. **Orange Olive Oil Cake**
   - Whisk eggs + sugar, stir in olive oil, add flour + baking powder, then fold in **orange zest and juice**.
   - Bake until a toothpick comes out clean; finish with an orange-juice glaze.

2. **Chocolate-Dipped Orange Slices**
   - Slice oranges into rounds, pat dry, dip halfway in melted dark chocolate.
   - Sprinkle flaky salt or chopped pistachios; chill to set.

3. **Orange Curd (for toast, tarts, yogurt)**
   - Cook **orange juice + zest + sugar + eggs** over low heat until thick; whisk in butter.
   - Chill; use as tart filling or swirl into yogurt.

4. **Orange Marmalade**
   - Thinly slice peel + fruit, simmer with sugar and water until gelled.
   - Great on biscuits, or as a glaze for meats.

## Savory recipes
5. **Orange & Fennel Salad**
   - Combine orange segments, shaved fennel, red onion, olive oil, salt, pepper.
   - Add olives or arugula; top with toasted almonds.

6. **Orange-Glazed Salmon (or tofu)**
   - Reduce **orange juice** with soy sauce, garlic, ginger, and a little honey until syrupy.
   - Brush on salmon/tofu and roast or pan-sear.

7. **Sheet-Pan Chicken with Orange + Rosemary**
   - Toss chicken thighs with orange slices, rosemary, olive oil, salt, pepper.
   - Roast; the orange caramelizes and flavors the pan juices.

8. **Orange Vinaigrette**
   - Shake **orange juice + zest**, olive oil, Dijon, honey, salt.
   - Perfect for spinach salads, grilled shrimp, or grain bowls.

## Drinks & quick uses
9. **Fresh Orange Smoothie**
   - Blend peeled orange with banana, Greek yogurt (or milk), ice, and a pinch of salt.

10. **Orange Agua Fresca / Sparkling Orange**
   - Mix fresh orange juice with cold water or sparkling water; sweeten to taste.

If you tell me what you’re making (dessert vs dinner), and whether you have **multiple oranges** or just one, I can suggest the best 2–3 options with exact ingredient amounts.
```

### Part 2: Local Image Processing
We then updated the code to read a local image (`mystery-fruit.jpeg`), encode it in `base64`, and send it via a `data_url`.

```python
# Get a response to image input
image_path = Path("mystery-fruit.jpeg")
image_format = "jpeg"
with open(image_path, "rb") as image_file:
     image_data = base64.b64encode(image_file.read()).decode("utf-8")

data_url = f"data:image/{image_format};base64,{image_data}"

response = client.responses.create(
     model=model_deployment,
     input=[
         {"role": "developer", "content": system_message},
         { "role": "user", "content": [  
             { "type": "input_text", "text": prompt},
             { "type": "input_image", "image_url": data_url}
         ]} 
     ]
)
print(response.output_text)
```

Running the app and asking `"What is this fruit? What recipes could I use it in?"` returned the following response:

```text
Ask a question about the image
(or type 'quit' to exit)
What is this fruit? What recipes could I use it in?
Getting a response ...

That’s **dragon fruit** (also called **pitaya**). This one is the **white-fleshed** type: bright pink skin with green “scales,” and white flesh dotted with tiny black seeds. Flavor is mild—lightly sweet, a bit like kiwi/pear.

## How to use it (quick prep)
- Slice in half lengthwise and **scoop out the flesh** with a spoon, or peel the skin off and cube the flesh.
- Eat chilled for best flavor.

## Recipe ideas
### 1) Dragon fruit smoothie
Blend: dragon fruit + banana + pineapple (or mango) + yogurt or coconut milk + ice + a squeeze of lime.

### 2) Fruit salad with lime-mint
Cube dragon fruit with strawberries, kiwi, and mango. Toss with **lime juice**, **honey**, and chopped **mint**.

### 3) Dragon fruit chia pudding
Mix: 2–3 Tbsp chia seeds + 1 cup milk (dairy or coconut) + a little sweetener. Fold in diced dragon fruit (or blend some in) and chill 4+ hours.

### 4) Salsa for fish or tacos
Dice dragon fruit + avocado + red onion + cilantro + jalapeño + lime + salt. Great over shrimp, mahi-mahi, or chicken.

### 5) Sorbet / popsicles
Blend dragon fruit with lime juice and simple syrup (or honey). Freeze as sorbet or pour into molds for popsicles.

### 6) Yogurt bowl / parfait
Layer Greek yogurt, granola, dragon fruit cubes, berries, and drizzle with honey.

### 7) Dragon fruit lemonade
Muddle/blend dragon fruit with lemon juice and water (sparkling is great), then sweeten to taste.

If you tell me whether you want **dessert, drink, or savory**, I can suggest a specific recipe with exact measurements.
```

**Status:** Completed successfully! 🎉
