# Assignment

## Brief

Write the Python codes for the following questions.

## Instructions

Paste the answer as Python in the answer code section below each question.

### Question 1

Question: From the `movies` collection, return the documents with the `plot` that starts with `"war"` in acending order of released date, print only title, plot and released fields. Limit the result to 5.

Answer:

```python
import pymongo
client = pymongo.MongoClient("mongodb+srv://keeboonking:N7rKYPQluTL44MbZ@cluster0.0zfhbtq.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0")
from pymongo.mongo_client import MongoClient
from pymongo.server_api import ServerApi

# MongoDB connection URI
uri = "mongodb+srv://keeboonking:N7rKYPQluTL44MbZ@cluster0.0zfhbtq.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0"

# Create a new client and connect to the server
client = MongoClient(uri, server_api=ServerApi('1'))

# Send a ping to confirm a successful connection
try:
    client.admin.command('ping')
    print("Pinged your deployment. You successfully connected to MongoDB!")
except Exception as e:
    print(f"Ping failed: {e}")

# Access the desired database and collection
db = client["sample_mflix"]  # Replace with your actual database name
collection = db["movies"]

# Query: plot starts with "war", sorted by released date ascending, limited to 5 results
query = { "plot": { "$regex": "^war", "$options": "i" } }
projection = { "title": 1, "plot": 1, "released": 1, "_id": 0 }
cursor = collection.find(query, projection).sort("released", 1).limit(5)

# Print the results
for doc in cursor:
    print(doc)
Pinged your deployment. You successfully connected to MongoDB!
{'plot': 'Warrior/pacifist Princess Nausicaè desperately struggles to prevent two warring nations from destroying themselves and their dying planet.', 'title': 'Nausicaè of the Valley of the Wind', 'released': datetime.datetime(1984, 3, 11, 0, 0)}
{'plot': 'Warrior/pacifist Princess Nausicaè desperately struggles to prevent two warring nations from destroying themselves and their dying planet.', 'title': 'Nausicaè of the Valley of the Wind', 'released': datetime.datetime(1984, 3, 11, 0, 0)}
{'plot': 'Warlords Kagetora and Takeda each wish to prevent the other from gaining hegemony in feudal Japan. The two samurai leaders pursue one another across the countryside, engaging in massive ...', 'title': 'Heaven and Earth', 'released': datetime.datetime(1991, 2, 8, 0, 0)}
{'plot': 'Warning! This synopsis contains spoilers Bajo las estrellas (beneath the stars) features the selfish...', 'title': 'Under the Stars', 'released': datetime.datetime(2007, 6, 15, 0, 0)}
{'plot': 'Warring alien and predator races descend on a small town, where unsuspecting residents must band together for any chance of survival.', 'title': 'Aliens vs. Predator: Requiem', 'released': datetime.datetime(2007, 12, 25, 0, 0)}
```

### Question 2

Question: Group by `rated` and count the number of movies in each.

Answer:

```python
# Access the desired database and collection
db = client["sample_mflix"]  # Replace with your actual database name
collection = db["movies"]

# Aggregation pipeline: group by 'rated' and count
pipeline = [
    { "$group": { "_id": "$rated", "count": { "$sum": 1 } } },
    { "$sort": { "count": -1 } }  # Optional: sort by count descending
]

# Execute the aggregation
results = collection.aggregate(pipeline)

# Print results
for result in results:
    print(f"Rated: {result['_id']}, Count: {result['count']}")

Rated: None, Count: 9895
Rated: R, Count: 5537
Rated: PG-13, Count: 2321
Rated: PG, Count: 1852
Rated: APPROVED, Count: 709
Rated: G, Count: 477
Rated: PASSED, Count: 181
Rated: TV-14, Count: 89
Rated: TV-PG, Count: 76
Rated: TV-MA, Count: 60
Rated: TV-G, Count: 59
Rated: GP, Count: 44
Rated: M, Count: 37
Rated: Approved, Count: 5
Rated: AO, Count: 3
Rated: TV-Y7, Count: 3
Rated: Not Rated, Count: 1
Rated: OPEN, Count: 1
```

### Question 3

Question: Count the number of movies with 3 comments or more.

Answer:

```python
# Access the database and the comments collection
db = client["sample_mflix"]  # Make sure this is the correct database name
comments_collection = db["comments"]  # This should be the collection storing comments

# Aggregation pipeline to count movies with 3 or more comments
pipeline = [
    { "$group": { "_id": "$movie_id", "comment_count": { "$sum": 1 } } },
    { "$match": { "comment_count": { "$gte": 3 } } },
    { "$count": "movies_with_3_or_more_comments" }
]

# Execute the aggregation
results = comments_collection.aggregate(pipeline)

# Print the result
for result in results:
    print(f"Number of movies with 3 or more comments: {result['movies_with_3_or_more_comments']}")

Number of movies with 3 or more comments: 400
```

## Submission

- Submit the URL of the GitHub Repository that contains your work to NTU black board.
- Should you reference the work of your classmate(s) or online resources, give them credit by adding either the name of your classmate or URL.
