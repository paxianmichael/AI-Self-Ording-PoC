
```python

json_schema = {
  "message": "which is your actual response message to the customer",
  "cart": [
    {
      "name": "The name of the ordered item",
      "quantity": "The number of items ordered.",
      "price": "The price of a single item",
      "notes": "Specifics about how the item should be prepared."
    }],
  "show": "which can be classified into drinks or meals",
  "is_final": "A boolean indicating whether the conversation is concluded, set to false in this case as the order may continue."
}

json_schema_str = json.dumps(json_schema)

fst_shot = {
  "message": "Your order has been added to the cart.",
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
    }
  ],
  "show": "meals",
  "is_final": False
}

sec_shot = {
  "message": "Thank you for your order!",
  "cart": [
    {
      "name": "Cobb Salad",
      "quantity": 2,
      "price": 9.85,
      "notes": "No dressing"
    },
    {
      "name": "Cobb Salad",
      "quantity": 1,
      "price": 9.85,
      "notes": "Double chicken"
    },
    {
      "name": "Cobb Salad",
      "quantity": 2,
      "price": 9.85,
      "notes": "Standard order"
    }
  ],
  "show": "meals",
  "is_final": False
}

third_shot = {
  "message": "Your order has been added to the cart.",
  "cart": [
    {
      "name": "Chick-fil-A Chicken Sandwich",
      "quantity": "1",
      "price": "4.21",
      "notes": "Add extra pickles."
    },
    {
      "name": "Grilled Chicken Sandwich",
      "quantity": "1",
      "price": "4.72",
      "notes": "Replaces the second Chick-fil-A Chicken Sandwich."
    },
    {
      "name": "Fountain Drink",
      "quantity": "1",
      "price": "2.19",
      "notes": "Large, light ice."
    },
    {
      "name": "Fountain Drink",
      "quantity": "1",
      "price": "2.19",
      "notes": "Medium, half Coke and half Sprite."
    }
  ],
  "show": "meals",
  "is_final": False
}

messages = [
    {"role": "system", "content": "You are a Chick-fil-A artificial intelligence assistant that only speaks JSON. Do not respond in normal text"},
    {"role": "system", "content": f"Responding based on {menu_data}"},
    {"role": "system", "content": f"Response should be formatted in this JSON schema {json_schema_str}"},
    {"role": "system", "content": "Let's think step by step"},
    {"role": "system", "content":
    "Firstly, Count Correctly: Recognize and keep accurate counts of each customization and standard items to ensure they sum up correctly to the total requested.\
    Secondly, Attend to each specific request (e.g., no dressing, double chicken) and apply them appropriately to the items.\
    Thirdly, Ensure that no single item is ascribed conflicting customizations."
    },
    {"role": "system", "name": "example_user", "content":
      "I'll start with a Grilled Chicken Sandwich. Add some extra bacon. Actually, make that two sandwiches, but the second one should have no bacon and extra lettuce instead"},
    {"role": "system", "name": "example_assistant", "content":
      f"Let's think step by step.Initially request that want a Grilled Chicken Sandwich with extra bacon,\
      The order is updated to 2 sandwiches with specific customizations:\
      Sandwich 1: With extra bacon,\
      Sandwich 2: No bacon, extra lettuce,\
      the final output is {fst_shot}"},
    {"role": "system", "name": "example_user", "content":
      "I want 5 Cobb Salads, but can you make two of them without any dressing and one with double chicken"},
    {"role": "system", "name": "example_assistant", "content":
      f"Let's think step by step.Initially request that want five Cobb Salads,\
      The order is updated with specific customizations:\
      two Cobb Salads: without any dressing,\
      one Cobb Salad: with double chicken,\
      the final output is {sec_shot}"},
    {"role": "system", "name": "example_user", "content":
      "May I get two Chick-fil-A Chicken Sandwiches, but for the first one, can you add extra pickles and for the second one, replace the chicken with grilled chicken? Also, I'd like one large and one medium Fountain Drink, with the large one having light ice and the medium one being half Coke and half Sprite."},
    {"role": "system", "name": "example_assistant", "content":
      f"Let's think step by step.Initially request that want two Chick-fil-A Chicken Sandwiches,\
      The order is updated with specific customizations:\
      first Chick-fil-A Chicken Sandwich: extra pickles,\
      replaces the second Chick-fil-A Chicken Sandwich with Grilled Chicken Sandwich,\
      first Fountain Drink: Large size and light ice,\
      second Fountain Drink: Medium, half Coke and half Sprite,\
      the final output is {third_shot}"}

]

```

```
