# coding-project-template

This project is a microservice to handle basic CRUD operations of songs from MongoDB database (a NoSQL database).


In Skills Network Labs:

1. Set up development environment:
   
```
bash ./bin/setup.sh
exit
```

2. Start MongoDB Server. Note down the host, port, and password (needed for the following command).

3. Run the flask server:`MONGODB_SERVICE=${localhost} MONGODB_USERNAME=root MONGODB_PASSWORD=${password} flask run --reload --debugger`

4. Test the RESTful APIs using curl
   
```
curl --request GET -i -w '\n' --url http://localhost:5000/song
curl --request GET -i -w '\n' --url http://localhost:5000/song/1
curl --request POST \
  -i -w '\n' \
  --url http://localhost:5000/song \
  --header 'Content-Type: application/json' \
  --data '{
        "id": 323,
        "lyrics": "Integer tincidunt ante vel ipsum. Praesent blandit lacinia erat. Vestibulum sed magna at nunc commodo placerat.\n\nPraesent blandit. Nam nulla. Integer pede justo, lacinia eget, tincidunt eget, tempus vel, pede.",
        "title": "in faucibus orci luctus et ultrices"
    }'
curl --request PUT \
  -i -w '\n' \
  --url http://localhost:5000/song/1 \
  --header 'Content-Type: application/json' \
  --data '{
        "lyrics": "yay hey yay yay",
        "title": "yay song"
    }'
curl --request DELETE \
  -i -w '\n' \
  --url http://localhost:5000/song/14 \
  --header 'Content-Type: "application/json"'
```
