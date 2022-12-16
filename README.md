# spoonacular_api

Planning a meal can be a challenge with the constraint on time and budget. To save time and money, this package helps find recipes and ingredients, and create a grocery list with a cost estimate using Spoonacular API.

## Installation

```bash
$ pip install spoonacular_api
```

## Usage

### 1. Getting API Key
- The user needs to sign up for an account on [`RapidAPI`](https://spoonacular.com/food-api/console#Dashboard) to use Spoonacular, and retrieve the API Key from `Profile` -> `Show / Hide API Key`
- The freemium model of Spoonacular offers 150 daily quota points to use for free.  Each method in this package uses about 1-2 points for each search
- If you need more quotas, the pricing is available at [`Pricing`](https://spoonacular.com/food-api/pricing)

### 2. Finding the recipe
- The user inputs the recipe name and ingredients they want to include in the parameters
- The method returns the top 5 recipes sorted by popularity in the form of a dataframe with the recipe name, the amount of servings, the price per serving, and the recipe id
        
### 3. Finding the ingredients
- The user inputs the recipe ID from `searchRecipe` method to search for ingredients of the recipe
- The method returns the list of ingredients, amount and unit

### 4. Adding/ deleting ingredients to the shopping list
- The user will use the username and hash generated when they initialize the class to perform these methods
- The user can choose to add the ingredients which returns a shopping list with:
    1. `addIngredient` method to add individually with the item name, amount, and unit
    2. `addAllIngredients` method to add all the ingredients from a recipe with the recipe ID
- The user can use `deleteItem` method to delete specific ingredients from the shopping list with the item IDs which will return an updated dataframe.

### 5. Creating a shopping list
- The method returns the user's shopping list in the form of a dataframe with item name, amount, unit, cost (USD), and item ID 

## Examples

```bash
import spoonacular_api.spoonacular_api as sp

# Initialize the class
recipe = sp.GroceryList('cooluser', 'John', 'Doe', 'thecoolestuser@gmail.com', 'Your API_Key')

# Search for recipes
>>>recipe.searchRecipe('soup', 'chicken', 'carrot')
	Recipe						Servings	  Price Per Serving	  Recipe ID
0	Red Lentil Soup with Chicken and Turnips	8	           2.77	            	715415
1	Chipotle Black Bean Soup with Avocado Cream	8	           1.43	            	638741
2	Chicken Mulligatawny Soup			6	           2.48	            	638199
3	Classic Matzo Ball Soup				6	           2.07	            	639616
4	Light Greek Lemon Chicken Orzo Soup		8	           2.37	            	1098350

# Find the ingredients
>>>recipe.getIngredients(638741)
	Ingredient					Amount		Unit
0	avocado	                         		1.000	    	small
1	canned black beans	           	  	1.276	    	kgs
2	diced carrots	                  		256.000	    	g
3	chicken broth	                   		940.000	    	ml
4	canned chipotle peppers in adobo		198.447	    	g
5	fresh cilantro leaves	         		2.000	    	Tbsps
6	lemon juice	                   		1.000	    	Tbsp
7	olive oil	                       		2.000	    	Tbsps
8	diced onions	                    		320.000	    	g
9	sour cream	                        	57.500	    	ml

# Add the ingredients individually
>>>recipe.addIngredient('chicken broth', 940, 'ml')
Shopping list
	Item			Amount      Unit        Cost (USD)      	Item ID
0	chicken broth	   	940.0	    ml	        3.0	        	1400323
1	Total cost 					3.0	
        
>>>recipe.addIngredient('diced carrots', 256, 'g')
Shopping list
	Item			Amount      Unit        Cost (USD)      	Item ID
0	chicken broth	   	940.0	    ml	        3.0	        	1400323
1	diced carrots		256.0	    g	        0.45	      		1400325
2	Total cost			        	3.45
       
# Add all the ingredients from a recipe with the recipe ID
>>>recipe.addAllIngredients(638741)
Shopping list
	Item	                        	Amount          Unit        Cost (USD)		Item ID
0	canned black beans			1.276	        kg	        2.28		1400329
1	chicken broth	                   	940.0	        ml	        3.00		1400333
2	canned chipotle peppers in adobo	198.447	        g	        1.98		1400335
3	sour cream	                  	57.5	       	ml	        0.38		1400345
4	olive oil				2.0		Tbsps	        0.33		1400341
5	avocado					1.0		small	        1.50		1400327
6	diced carrots	                   	256.0	        g	        0.45		1400331
7	fresh cilantro leaves	            	2.0	   	Tbsps	        0.03		1400337
8	lemon juice	                    	1.0	       	Tbsp	        0.10		1400339
9	diced onions	                    	320.0	        g	        0.70		1400343
10	Total cost			          				10.76

# Delete ingredients from the shopping listwith the recipe ID
>>>recipe.deleteItem(1400329, 1400333, 1400335)
Shopping list
	Item	                        	Amount          Unit        Cost (USD)		Item ID
0	sour cream	                  	57.5	       	ml	        0.38		1400345
1	olive oil				2.0		Tbsps	        0.33		1400341
2	avocado					1.0		small	        1.50		1400327
3	diced carrots	                   	256.0	        g	        0.45		1400331
4	fresh cilantro leaves	            	2.0	   	Tbsps	        0.03		1400337
5	lemon juice	                    	1.0	       	Tbsp	        0.10		1400339
6	diced onions	                    	320.0	        g	        0.70		1400343
7	Total cost			          				3.43
        
# Get the shopping list
>>>recipe.getShoppingList()
Shopping list
	Item	                        	Amount          Unit        Cost (USD)		Item ID
0	canned black beans			1.276	        kg	        2.28		1400329
1	chicken broth	                   	940.0	        ml	        3.00		1400333
2	canned chipotle peppers in adobo	198.447	        g	        1.98		1400335
3	sour cream	                  	57.5	       	ml	        0.38		1400345
4	olive oil				2.0		Tbsps	        0.33		1400341
5	avocado					1.0		small	        1.50		1400327
6	diced carrots	                   	256.0	        g	        0.45		1400331
7	fresh cilantro leaves	            	2.0	   	Tbsps	        0.03		1400337
8	lemon juice	                    	1.0	       	Tbsp	        0.10		1400339
9	diced onions	                    	320.0	        g	        0.70		1400343
10	Total cost			          				10.76
```

## Contributing

Interested in contributing? Check out the contributing guidelines. Please note that this project is released with a Code of Conduct. By contributing to this project, you agree to abide by its terms.

## License

`spoonacular_api` was created by cookiecutter https://github.com/py-pkgs/py-pkgs-cookiecutter.git. It is licensed under the terms of the MIT license.

## Credits

`spoonacular_api` was created with [`Spoonacular API`](https://spoonacular.com/food-api), [`cookiecutter`](https://cookiecutter.readthedocs.io/en/latest/) and the `py-pkgs-cookiecutter` [template](https://github.com/py-pkgs/py-pkgs-cookiecutter).

