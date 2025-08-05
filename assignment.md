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
import re

# Connect to MongoDB
client = pymongo.MongoClient(
    "mongodb+srv://keeboonking:N7rKYPQluTL44MbZ@cluster0.0zfhbtq.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0",
    serverSelectionTimeoutMS=30000,
    socketTimeoutMS=120000,
    connectTimeoutMS=20000,
    maxPoolSize=10,
    retryWrites=True
)

# Access the movies collection
db = client['sample_mflix']
collection = db['movies']

# Query: Find plots that start with "war"
results = collection.find(
    {"plot": {"$regex": "^war", "$options": "i"}},  # case-insensitive match at the beginning
    {"title": 1, "plot": 1, "released": 1, "_id": 0}  # fields to return
).sort("released", 1).limit(5)

# Print results
for doc in results:
    print(doc)
```

### Question 2

Question: Group by `rated` and count the number of movies in each.

Answer:

```python
import pymongo

# Connect to MongoDB
client = pymongo.MongoClient(
    "mongodb+srv://keeboonking:N7rKYPQluTL44MbZ@cluster0.0zfhbtq.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0",
    serverSelectionTimeoutMS=30000,
    socketTimeoutMS=120000,
    connectTimeoutMS=20000,
    maxPoolSize=10,
    retryWrites=True
)

# Access the movies collection
db = client['sample_mflix']
collection = db['movies']

# Aggregation pipeline to group by "rated" and count
pipeline = [
    {"$group": {
        "_id": "$rated",             # Group by the "rated" field
        "count": {"$sum": 1}         # Count documents in each group
    }},
    {"$sort": {"count": -1}}         # Optional: sort by count descending
]

# Execute aggregation and print results
results = collection.aggregate(pipeline)

for doc in results:
    print(doc)
```

### Question 3

Question: Count the number of movies with 3 comments or more.

Answer:

```python
import pymongo

# Connect to MongoDB
client = pymongo.MongoClient(
    "mongodb+srv://keeboonking:N7rKYPQluTL44MbZ@cluster0.0zfhbtq.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0",
    serverSelectionTimeoutMS=60000,
    socketTimeoutMS=180000,
    connectTimeoutMS=40000,
    maxPoolSize=10,
    retryWrites=True
)

# Access the sample_mflix database and movies collection
db = client['sample_mflix']
collection = db['movies']

# Aggregation pipeline to count movies with at least 3 comments
pipeline = [
    {"$lookup": {
        "from": "comments",
        "localField": "_id",
        "foreignField": "movie_id",
        "as": "movie_comments"
    }},
    {"$match": {
        "$expr": {"$gte": [{"$size": "$movie_comments"}, 3]}
    }},
    {"$count": "movies_with_3_or_more_comments"}
]

# Execute and print result
result = list(collection.aggregate(pipeline))

print(client['sample_mflix']['movies'].find_one())
```

## Submission

- Submit the URL of the GitHub Repository that contains your work to NTU black board.
- Should you reference the work of your classmate(s) or online resources, give them credit by adding either the name of your classmate or URL.
