
```python

MODEL = "gpt-3.5-turbo"
openai.api_key = "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"
# key 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
delimiter = "####"
json_schema = {
    "messages": "response to the user",
    "cart": [
        {
        "name": "order item from user",
        "quantity": "numbers of item",
        "price": "corresponding price of each item",
        "notes": "extra requirements from user"
        }
    ]
}

json_schema_str = json.dumps(json_schema)

one_shot = {
  "cart": [
    {
      "name": "Grilled Chicken Sandwich",
      "quantity": "1",
      "price": "6.99",
      "notes": "Add extra bacon."
    },
    {
      "name": "Grilled Chicken Sandwich",
      "quantity": "1",
      "price": "6.99",
      "notes": "No bacon, add extra lettuce."
    }]
}

sec_shot = {
  "cart": [
    {
      "name": "Grilled Chicken Sandwich",
      "quantity": "2",
      "price": "4.72",
      "notes": "None"
    },
    {
      "name": "Waffle Potato FriesTM (large)",
      "quantity": "2",
      "price": "2.45",
      "notes": "None"
    },
    {
      "name": "Chick-fil-A Chicken Biscuit",
      "quantity": "1",
      "price": "5.89",
      "notes": "extra honey"
    },
    {
      "name": "Grilled Chicken Sandwich",
      "quantity": "1",
      "price": "4.72",
      "notes": "None"
    },
    {
      "name": "Grilled Nuggets",
      "quantity": "2",
      "price": "3.42",
      "notes": "None"
    }
  ]
}

third_shot = {
  "cart": [
    {
      "name": "Cobb Salad",
      "quantity": "2",
      "price": "9.85",
      "notes": "no dressing"
    },
    {
      "name": "Cobb Salad",
      "quantity": "1",
      "price": "9.85",
      "notes": "double chicken"
    },
    {
      "name": "Cobb Salad",
      "quantity": "2",
      "price": "9.85",
      "notes": "None"
    }
  ]
}

fourth_shot = {
  "cart": [
    {
      "name": "Chick-fil-A Chicken Mini (4 ct)",
      "quantity": "1",
      "price": "6.90",
      "notes": "None"
    },
    {
      "name": "Chick-fil-A Deluxe Sandwich",
      "quantity": "1",
      "price": "4.92",
      "notes": "None"
    }
  ]
}

messages = [
    {"role": "system", "content": "You are a helpful assistant that is capable of managing and taking complex food orders and only speaks JSON. Do not respond in normal text"},
    {"role": "system", "content": f"Responding based on {menu_data}"},
    {"role": "system", "content": f"Response should be formatted in this JSON schema {json_schema_str}"},
    {"role": "system", "content":
        """
        Step1: {delimiter} Identifying Items and Quantities: The sentences often start by mentioning a specific quantity and item,

        Step2: {delimiter} Order Modifications:
          - For modifications to existing items, add details to the `notes` field of that item. For example:
            {{"name": "Chick-fil-A Deluxe Sandwich", "notes": "No mayo"}}

          - For adding a new item which is on menu_data, create a new entry in `cart` with the item name, quantity, and price. For example:
            {{"name": "Sweet Tea", "quantity": 1, "price": 2.19, "notes": "Large}}

        Step3: {delimiter} Build final order:
          - For existing menu items, lookup the price based on {menu_data}
          - If the requested item is not on the menu, prompt the user to clarify or choose a valid item
          - Add the price to the cart entry along with the name and quantity
        """.format(delimiter=delimiter, menu_data=menu_data)
    },
    {"role": "system", "name": "example_user", "content":
      "I'll start with a Grilled Chicken Sandwich. Add some extra bacon. Actually, make that two sandwiches, but the second one should have no bacon and extra lettuce instead"},
    {"role": "system", "name": "example_assistant", "content":
      f"""Let's think step by step,\
      Initially request that want a Grilled Chicken Sandwich with extra bacon for customization,\
      Actually, make that two sandwiches meaning that two Grilled Chicken Sandwich will be added into cart with extra bacon for customization rather than one Grilled Sandwich with extra bacon,\
      but the second one should have no bacon and extra lettuce instead meaning that the second Grilled Chicken Sandwich with no bacon and extra lettuce for customization,\
      Therefore, totally want two orders of Grilled Chicken Sandwich, one with extra bacon and the one with no bacon and extra lettuce,\
      The final output is {one_shot}"""},
    {"role": "system", "name": "example_user", "content":
     "Let's order four combos. I want all of them to include a Grilled Chicken Sandwich. For the sides, the first two should come with Waffle Potato FriesTM (large) and the last two with Grilled Nuggets. Actually, on the third combo, replace the Grilled Chicken Sandwich with a Chick-fil-A Chicken Biscuit and add some extra honey"},
    {"role": "system", "name": "example_assistant", "content":
     f"""Let's think step by step,\
      Initially request four combos and each of the combos includes a Grilled Chicken Sandwich meaning that order four Grilled Chicken Sandwiches,\
      First two Grilled Chicken Sandwiches come with Waffle Potato FriesTM (Large) meaning that two Waffle Potato FriesTM (Large) will be added into cart,\
      Therefore, we have four Grilled Chicken Sandwiches and two Waffle Potato FriesTM (Large) added into cart so far,\
      the rest two Grilled Chicken Sandwiches come with Grilled Nuggets meaning that two Griled Nuggets will be added into cart,\
      on the third combo, replace the Grilled Chicken Sandwich with a Chick-fil-A Chicken Biscuit and add some extra honey meaning that removing one Grilled Chicken Sandwich from the cart and adding one Chick-fil-A Chicken Bisuit with extra honey for customization into cart,\
      Therefore,we have two orders of Grilled Chicken Sandwich and two orders of Waffle Potato FriesTM (Large),\
      one order of Chick-fil-A Chicken Biscuit with extra honey,\
      one order of Grilled Chicken Sandwich,\
      two orders of Grilled Nuggets ,\
      The final output is {sec_shot}"""},
    {"role": "system", "name": "example_user", "content":
     "I want 5 Cobb Salads, but can you make two of them without any dressing and one with double chicken"},
    {"role": "system", "name": "example_assistant", "content":
     f"""Let's think step by step,\
      Initially request five orders of Cobb Salads,\
      two of them without any dressing meaning that two Cobb Salads are without dressing for customization,\
      one with double chicken meaning that one Cobb Salad is with double chicken for customization,\
      Therefore, we have five orders of Cobb Salads totally,\
      The final output is {third_shot}"""},
    {"role":"system", "name": "example_user", "content":
     "I'll start with a Chick-fil-A Chicken Mini (4 ct). Actually, make that two and can you swap the second one for a Chick-fil-A Deluxe Sandwich?"},
    {"role":"system", "name": "example_assistant", "content":
     f"""Let's think step by step,\
     Initially want one Chick-fil-A Chicken Mini (4 ct),\
     Actually, make that two meaning that wanting two orders of Chick-fil-A Chicken Mini (4 ct) rather than one order,\
     swap the second one for a Chick-fil-A Deluxe Sandwich meaning that wanting one order of Chick-fil-A Chicken Mini (4 ct) and one order of Chick-fil-A Deluxe Sandwich instead of two orders of Chick-fil-A Chicken Mini (4 ct),\
     Therefore, we have one order of Chick-fil-A Chicken Mini (4 ct) and one order of Chick-fil-A Deluxe Sandwich,\
     The final output is {fourth_shot}"""}

  ]

```
