# Full Stack Trivia API Backend

## Getting Started

### Installing Dependencies

#### Python 3.7

Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python)

#### Virtual Enviornment

We recommend working within a virtual environment whenever using Python for projects. This keeps your dependencies for each project separate and organaized. Instructions for setting up a virual enviornment for your platform can be found in the [python docs](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)

#### PIP Dependencies

Once you have your virtual environment setup and running, install dependencies by naviging to the `/backend` directory and running:

```bash
pip install -r requirements.txt
```

This will install all of the required packages we selected within the `requirements.txt` file.

##### Key Dependencies

- [Flask](http://flask.pocoo.org/)  is a lightweight backend microservices framework. Flask is required to handle requests and responses.

- [SQLAlchemy](https://www.sqlalchemy.org/) is the Python SQL toolkit and ORM we'll use handle the lightweight sqlite database. You'll primarily work in app.py and can reference models.py. 

- [Flask-CORS](https://flask-cors.readthedocs.io/en/latest/#) is the extension we'll use to handle cross origin requests from our frontend server. 

## Database Setup
With Postgres running, restore a database using the trivia.psql file provided. From the backend folder in terminal run:
```bash
psql trivia < trivia.psql
```

## Running the server

From within the `backend` directory first ensure you are working using your created virtual environment.

To run the server, execute:

```bash
export FLASK_APP=flaskr
export FLASK_ENV=development
flask run
```

Setting the `FLASK_ENV` variable to `development` will detect file changes and restart the server automatically.

Setting the `FLASK_APP` variable to `flaskr` directs flask to use the `flaskr` directory and the `__init__.py` file to find the application. 

## Tasks

One note before you delve into your tasks: for each endpoint you are expected to define the endpoint and response data. The frontend will be a plentiful resource because it is set up to expect certain endpoints and response data formats already. You should feel free to specify endpoints in your own way; if you do so, make sure to update the frontend or you will get some unexpected behavior. 

1. Use Flask-CORS to enable cross-domain requests and set response headers. 
2. Create an endpoint to handle GET requests for questions, including pagination (every 10 questions). This endpoint should return a list of questions, number of total questions, current category, categories. 
3. Create an endpoint to handle GET requests for all available categories. 
4. Create an endpoint to DELETE question using a question ID. 
5. Create an endpoint to POST a new question, which will require the question and answer text, category, and difficulty score. 
6. Create a POST endpoint to get questions based on category. 
7. Create a POST endpoint to get questions based on a search term. It should return any questions for whom the search term is a substring of the question. 
8. Create a POST endpoint to get questions to play the quiz. This endpoint should take category and previous question parameters and return a random questions within the given category, if provided, and that is not one of the previous questions. 
9. Create error handlers for all expected errors including 400, 404, 422 and 500. 


GET `/categories`

curl -X GET http://127.0.0.1:5000/categories
- Fetches a dictionary of categories in which the keys are the ids and the value is the corresponding string of the category
- Request Arguments: None
- Returns: An object with a single key, categories, that contains a object of id: category_string key:value pairs.
```
{
  "categories": {
    "1": "Science", 
    "2": "Art", 
    "3": "Geography", 
    "4": "History", 
    "5": "Entertainment", 
    "6": "Sports"
  }, 
  "success": true, 
  "total_categories": 6
}
```



GET `\questions` 

curl -X GET http://127.0.0.1:5000/questions 
- Get a paginated dictionary of questions of all categories
- *Request Arguments (optional):* page:int (defult value is 1)
- *Returns:*  
``` 
{
  "categories": {
    "1": "Science", 
    "2": "Art", 
    "3": "Geography", 
    "4": "History", 
    "5": "Entertainment", 
    "6": "Sports"
  }, 
  "questions": [
    {
      "answer": "Apollo 13", 
      "category": 5, 
      "difficulty": 4, 
      "id": 2, 
      "question": "What movie earned Tom Hanks his third straight Oscar nomination, in 1996?"
    },...
    ...
    ...

```

DELETE `/questions/<id>`

curl -X DELETE http://127.0.0.1:5000/questions/71
- Delete the question in database if its id match with given id
- *Request Arguments:* int:id 
- *Returns:* 
```
{
  "deleted": 71, 
  "success": true, 
  "total_questions": 41
}
```

POST `/questions`

curl -X POST -H "Content-Type: application/json" -d '{"question": "what is the best country", "answer": "Saudi Arabia", "difficulty": 1, "category": 1}' http://127.0.0.1:5000/questions
- Add a new question in database 
- *Request body:* {question:string, answer:string, difficulty:int, category:string}
- *Returns:* 
```
{
  "created": 71, 
  "success": true, 
  "total_questions": 42
}
```
POST `/questions/search`

curl -X POST -H "Content-Type: application/json" -d'{"search_term":"tournament"}' http://127.0.0.1:5000/questions/search
- Get all questions where given substring matches with the search_term 
- *Request body:* jasonbody{'search_term':'string'}
- *Returns:*
```
{
  "questions": [
    {
      "answer": "Brazil", 
      "category": 6, 
      "difficulty": 3, 
      "id": 10, 
      "question": "Which is the only team to play in every soccer World Cup tournament?"
    }
  ], 
  "success": true, 
  "total_questions": 1
}
```

GET `/categories/<int:category_id>/questions`

curl -X GET http://127.0.0.1:5000/categories/6/questions
- Get question with a given category
- *Request argument:* category_id:int
- *Returns:*
```
{
  "questions": [
    {
      "answer": "Brazil", 
      "category": 6, 
      "difficulty": 3, 
      "id": 10, 
      "question": "Which is the only team to play in every soccer World Cup tournament?"
    }
  ], 
  "success": true, 
  "total_questions": 1
}
```
POST `/play`

curl -X POST -H "Content-Type: application/json" -d '{"previous_questions":[],"quiz_category":{"type":"Science","id":"1"}}' http://127.0.0.1:5000/play
- Get random question within a given category.  
- *Request body:* {previous_questions: arr, quiz_category: {id:int, type:string}}
- *Returns*: 
```
{
  "question": {
    "answer": "Saudi Arabia", 
    "category": 1, 
    "difficulty": 1, 
    "id": 33, 
    "question": "what is the best country"
  }, 
  "success": true
}
```

## Error Handling
Error returning a json object in the following format:
```
{
  "error": 404, 
  "message": "resource not found", 
  "success": false
}

The APi will return different type of errors, if request failed:
400:bad request
404:resource not found
422:unprocessable
```

## Testing
To run the tests, run
```
dropdb trivia_test
createdb trivia_test
psql trivia_test < trivia.psql
python test_flaskr.py
```