# Substitution of cooking ingredients based on recipe context

Before running the ipynb notebooks clone the main directory to your desired workspace as follows

## For FoodBERT

1) Download the food bert model and store the files in the folder ./Food_BERT_model/checkpoint(If the link dosent work please Email at ge65yuf@mytum.de) - https://syncandshare.lrz.de/getlink/fi3s7NZRbc31SVkCguJth8fc/checkpoint-final

2) Download the layer1.json file from Recipe1M dataset and put it inside ./data folder.

3) Download the recipes_with_nutritional_info.json file from Recipe1M dataset and put it inside ./Nutrition data folder.

## For ELMo

1) Download the notebooks foodelmo_embedding.ipynb *to generate recipe embeddings* and foodelmo_model.ipynb *to use the model*

2) Download the data, yummly_ingredients and the ELMo_embedding and ELMo_model folders as prerequisites to the notebooks

3) The sequence of running the ELMo code is:
    1. foodelmo_embedding.ipynb
    2. foodelmo_model.ipynb

### Detail of each folder

#### data
Contain code to read layer1.json and create 10 different files each with 100k Recipes.

#### yummly_ingredient
code to clean and read ingredients_yummly.json file and create an ingredient list from it.

#### Food_BERT_model 
Contains all the files of food bert model including extended vocabulary and ingredient list.

#### Base_Bert_Embeddings/Food_Bert_Embeddings

Contains dictionary as pickle files that store the contextual embeddings of ingredients present in ingredient list using base bert and food bert respectively.

#### Nutrition data
containg code to read Nutrition information given by Recipe1M daaset into a dictonary file and store it as a pickle file which can be used to find the nutrition information of the available ingredients. 

#### ELMo_embedding
Directory consists of dependencies to run *foodelmo_embedding.ipynb* for generating contextual embeddings. Further prerequisites needed such as additional data can be pushed here.

#### ELMo_model
Directory to store the pickle files of the embeddings created in the ELMo embedding generation process by *foodelmo_embedding.ipynb*. Pickles are generated on a frequency basis, to store progress.
Also used by the *foodelmo_model.ipynb* to retrieve pickled files and use for prediction.

#### Testing_BERT
Contain code to test BERT approaches and generate substitutes.

### Detail of files

#### Bert_base_embeddings.ipynb/Food_Bert_embeddings.ipynb

Contains code to generate contextual word embedding for each ingredient in ingredient list with base bert and food bert respectively. It uses 3 different techniques explained in the report.

#### foodelmo_embedding.ipynb
- .ipynb notebook that consists of code to generate summed contextual word embedding for each ingredient that is detected in the set of recipes imported in the notebook. Embeddings generated from a language model trained on the 1 Billion word benchmark.
- Exports two pickle files to use for *foodelmo_model.ipynb* for inferencing:
    1. ingredient_embed_dict.pkl: A pkl file, of a dict consisting of a sum of the respective embeddings of each of the ingredient found in the recipe, where the key denotes the ingredient and the value denotes the summed up embedding value of dimension 1024.
    2. ingredient_freq.pkl: A pkl file, of a dict consisting of the number of occurances of a particular ingredient for all recipes processed by the ELMo model.
- Performance observed for embedding generation: ~15 sec per recipe on CPU.

#### foodelmo_model.ipynb
- .ipynb notebook that consists of the code to predict the closest ingredient for a test ingredient/sentence/recipe.
- Imports the pkl files created above to use for embedding calculations.
- The embedding value for an ingredient from *ingredient_embed_dict* is divided by its corresponding value from *ingredient_freq* to find the corresponding mean value of the embedding.
