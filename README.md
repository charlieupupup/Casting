# Capstone Project

![Python applicion](https://github.com/charlieupupup/Casting/workflows/Python%20application/badge.svg)

## content

1. Database modeling with `postgres` & `sqlalchemy`
2. API for CRUD on database
3. Testing with `Unittest`
4. Auth & Role based auth with `Auth0`
5. Deployment with `Heroku`

## local dev

enable venv

```sh
python3 -m venv .
```

install dependencies

```sh
pip3 install -r requirements.txt
```

db config `config.py`

```python
database_setup = {
    "database_name_production" : "agency",
    "user_name" : "postgres",
    "password" : "testpassword123",
    "port" : "localhost:5432"
}
```

set up `Auth0`

run dev server

```sh
python3 app.py
```

testing

```sh
python3 test.py
```

## API

<https://artist-capstone-fsnd-matthew.herokuapp.com>

1. Actors
   1. [GET /actors](#get-actors)
   2. [POST /actors](#post-actors)
   3. [DELETE /actors](#delete-actors)
   4. [PATCH /actors](#patch-actors)
2. Movies
   1. [GET /movies](#get-movies)
   2. [POST /movies](#post-movies)
   3. [DELETE /movies](#delete-movies)
   4. [PATCH /movies](#patch-movies)

Following section includes

1. Normal response

2. Error Handling

### 1. GET /actors

Query paginated actors.

```bash
$ curl -X GET https://artist-capstone-fsnd-matthew.herokuapp.com/actors?page1
```

- Fetches a list of dictionaries. Keys are the ids plus possible fields

- Request Arguments: 
    - **integer** `page` (optional, 10 actors per page, defaults to `1` if not given)
- Request Headers: **None**
- Requires permission: `read:actors`
- Returns: 
  1. List of dict of actors with following fields:
      - **integer** `id`
      - **string** `name`
      - **string** `gender`
      - **integer** `age`
  2. **boolean** `success`

#### Example response
```js
{
  "actors": [
    {
      "age": 25,
      "gender": "Male",
      "id": 1,
      "name": "Matthew"
    }
  ],
  "success": true
}
```
#### Errors
If you try fetch a page which does not have any actors, you will encounter an error which looks like this:

```bash
$ curl -X GET https://artist-capstone-fsnd-matthew.herokuapp.com/actors?page123124
```

will return

```js
{
  "error": 404,
  "message": "no actors found in database.",
  "success": false
}
```

# <a name="post-actors"></a>
### 2. POST /actors

Insert new actor into database.

```bash
$ curl -X POST https://artist-capstone-fsnd-matthew.herokuapp.com/actors
```

- Request Arguments: **None**
- Request Headers: (_application/json_)
       1. **string** `name` (<span style="color:red">*</span>required)
       2. **integer** `age` (<span style="color:red">*</span>required)
       3. **string** `gender`
- Requires permission: `create:actors`
- Returns: 
  1. **integer** `id from newly created actor`
  2. **boolean** `success`

#### Example response
```js
{
    "created": 5,
    "success": true
}

```
#### Errors
If you try to create a new actor without a requiered field like `name`,
it will throw a `422` error:

```bash
$ curl -X GET https://artist-capstone-fsnd-matthew.herokuapp.com/actors?page123124
```

will return

```js
{
  "error": 422,
  "message": "no name provided.",
  "success": false
}
```

# <a name="patch-actors"></a>
### 3. PATCH /actors

Edit an existing Actor

```bash
$ curl -X PATCH https://artist-capstone-fsnd-matthew.herokuapp.com/actors/1
```

- Request Arguments: **integer** `id from actor you want to update`
- Request Headers: (_application/json_)
       1. **string** `name` 
       2. **integer** `age` 
       3. **string** `gender`
- Requires permission: `edit:actors`
- Returns: 
  1. **integer** `id from updated actor`
  2. **boolean** `success`
  3. List of dict of actors with following fields:
      - **integer** `id`
      - **string** `name`
      - **string** `gender`
      - **integer** `age`

#### Example response
```js
{
    "actor": [
        {
            "age": 30,
            "gender": "Other",
            "id": 1,
            "name": "Test Actor"
        }
    ],
    "success": true,
    "updated": 1
}
```
#### Errors
If you try to update an actor with an invalid id it will throw an `404`error:

```bash
$ curl -X PATCH https://artist-capstone-fsnd-matthew.herokuapp.com/actors/125
```

will return

```js
{
  "error": 404,
  "message": "Actor with id 125 not found in database.",
  "success": false
}
```
Additionally, trying to update an Actor with already existing field values will result in an `422` error:

```js
{
  "error": 422,
  "message": "provided field values are already set. No update needed.",
  "success": false
}
```

# <a name="delete-actors"></a>
### 4. DELETE /actors

Delete an existing Actor

```bash
$ curl -X DELETE https://artist-capstone-fsnd-matthew.herokuapp.com/actors/1
```

- Request Arguments: **integer** `id from actor you want to delete`
- Request Headers: `None`
- Requires permission: `delete:actors`
- Returns: 
  1. **integer** `id from deleted actor`
  2. **boolean** `success`

#### Example response
```js
{
    "deleted": 5,
    "success": true
}

```
#### Errors
If you try to delete actor with an invalid id, it will throw an `404`error:

```bash
$ curl -X DELETE https://artist-capstone-fsnd-matthew.herokuapp.com/actors/125
```

will return

```js
{
  "error": 404,
  "message": "Actor with id 125 not found in database.",
  "success": false
}
```

# <a name="get-movies"></a>
### 5. GET /movies

Query paginated movies.

```bash
$ curl -X GET https://artist-capstone-fsnd-matthew.herokuapp.com/movies?page1
```
- Fetches a list of dictionaries of examples in which the keys are the ids with all available fields
- Request Arguments: 
    - **integer** `page` (optional, 10 movies per page, defaults to `1` if not given)
- Request Headers: **None**
- Requires permission: `read:movies`
- Returns: 
  1. List of dict of movies with following fields:
      - **integer** `id`
      - **string** `name`
      - **date** `release_date`
  2. **boolean** `success`

#### Example response
```js
{
  "movies": [
    {
      "id": 1,
      "release_date": "Sun, 16 Feb 2020 00:00:00 GMT",
      "title": "Matthew first Movie"
    }
  ],
  "success": true
}

```
#### Errors
If you try fetch a page which does not have any movies, you will encounter an error which looks like this:

```bash
$ curl -X GET https://artist-capstone-fsnd-matthew.herokuapp.com/movies?page123124
```

will return

```js
{
  "error": 404,
  "message": "no movies found in database.",
  "success": false
}
```

# <a name="post-movies"></a>
### 6. POST /movies

Insert new Movie into database.

```bash
$ curl -X POST https://artist-capstone-fsnd-matthew.herokuapp.com/movies
```

- Request Arguments: **None**
- Request Headers: (_application/json_)
       1. **string** `title` (<span style="color:red">*</span>required)
       2. **date** `release_date` (<span style="color:red">*</span>required)
- Requires permission: `create:movies`
- Returns: 
  1. **integer** `id from newly created movie`
  2. **boolean** `success`

#### Example response
```js
{
    "created": 5,
    "success": true
}
```
#### Errors
If you try to create a new movie without a requiered field like `name`,
it will throw a `422` error:

```bash
$ curl -X GET https://artist-capstone-fsnd-matthew.herokuapp.com/movies?page123124
```

will return

```js
{
  "error": 422,
  "message": "no name provided.",
  "success": false
}
```

# <a name="patch-movies"></a>
### 7. PATCH /movies

Edit an existing Movie

```bash
$ curl -X PATCH https://artist-capstone-fsnd-matthew.herokuapp.com/movies/1
```

- Request Arguments: **integer** `id from movie you want to update`
- Request Headers: (_application/json_)
       1. **string** `title` 
       2. **date** `release_date` 
- Requires permission: `edit:movies`
- Returns: 
  1. **integer** `id from updated movie`
  2. **boolean** `success`
  3. List of dict of movies with following fields:
        - **integer** `id`
        - **string** `title` 
        - **date** `release_date` 

#### Example response
```js
{
    "created": 1,
    "movie": [
        {
            "id": 1,
            "release_date": "Sun, 16 Feb 2020 00:00:00 GMT",
            "title": "Test Movie 123"
        }
    ],
    "success": true
}

```
#### Errors
If you try to update an movie with an invalid id it will throw an `404`error:

```bash
$ curl -X PATCH https://artist-capstone-fsnd-matthew.herokuapp.com/movies/125
```

will return

```js
{
  "error": 404,
  "message": "Movie with id 125 not found in database.",
  "success": false
}
```
Additionally, trying to update an Movie with already existing field values will result in an `422` error:

```js
{
  "error": 422,
  "message": "provided field values are already set. No update needed.",
  "success": false
}
```

# <a name="delete-movies"></a>
### 8. DELETE /movies

Delete an existing movie

```bash
$ curl -X DELETE https://artist-capstone-fsnd-matthew.herokuapp.com/movies/1
```

- Request Arguments: **integer** `id from movie you want to delete`
- Request Headers: `None`
- Requires permission: `delete:movies`
- Returns: 
  1. **integer** `id from deleted movie`
  2. **boolean** `success`

#### Example response
```js
{
    "deleted": 5,
    "success": true
}

```
#### Errors
If you try to delete movie with an invalid id, it will throw an `404`error:

```bash
$ curl -X DELETE https://artist-capstone-fsnd-matthew.herokuapp.com/movies/125
```

will return

```js
{
  "error": 404,
  "message": "Movie with id 125 not found in database.",
  "success": false
}
```

## Authentification

All API Endpoints are decorated with Auth0 permissions. To use the project locally, you need to config Auth0 accordingly

### Auth0 for locally use
#### Create an App & API

1. Login to https://manage.auth0.com/ 
2. Click on Applications Tab
3. Create Application
4. Give it a name like `Music` and select "Regular Web Application"
5. Go to Settings and find `domain`. Copy & paste it into config.py => auth0_config['AUTH0_DOMAIN'] (i.e. replace `"example-matthew.eu.auth0.com"`)
6. Click on API Tab 
7. Create a new API:
   1. Name: `Music`
   2. Identifier `Music`
   3. Keep Algorithm as it is
8. Go to Settings and find `Identifier`. Copy & paste it into config.py => auth0_config['API_AUDIENCE'] (i.e. replace `"Example"`)

#### Create Roles & Permissions

1. Before creating `Roles & Permissions`, you need to `Enable RBAC` in your API (API => Click on your API Name => Settings = Enable RBAC => Save)
2. Also, check the button `Add Permissions in the Access Token`.
2. First, create a new Role under `Users and Roles` => `Roles` => `Create Roles`
3. Give it a descriptive name like `Casting Assistant`.
4. Go back to the API Tab and find your newly created API. Click on Permissions.
5. Create & assign all needed permissions accordingly 
6. After you created all permissions this app needs, go back to `Users and Roles` => `Roles` and select the role you recently created.
6. Under `Permissions`, assign all permissions you want this role to have. 

# <a name="authentification-bearer"></a>
### Auth0 to use existing API
If you want to access the real, temporary API, bearer tokens for all 3 roles are included in the `config.py` file.

## Existing Roles

They are 3 Roles with distinct permission sets:

1. Casting Assistant:
  - GET /actors (view:actors): Can see all actors
  - GET /movies (view:movies): Can see all movies
2. Casting Director (everything from Casting Assistant plus)
  - POST /actors (create:actors): Can create new Actors
  - PATCH /actors (edit:actors): Can edit existing Actors
  - DELETE /actors (delete:actors): Can remove existing Actors from database
  - PATCH /movies (edit:movies): Can edit existing Movies
3. Exectutive Dircector (everything from Casting Director plus)
  - POST /movies (create:movies): Can create new Movies
  - DELETE /movies (delete:movies): Can remove existing Motives from database

In your API Calls, add them as Header, with `Authorization` as key and the `Bearer token` as value. Don´t forget to also
prepend `Bearer` to the token (seperated by space).

For example: (Bearer token for `Executive Director`)
```js
{
    "Authorization": "Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6Ik16azVRVUk0TXpSR04wSXhOVU13TkRrME16QXdNMFpHTmtFMU1VWXdPRUpCTmpnMFJrVTBSZyJ9.eyJpc3MiOiJodHRwczovL2ZzbmQtbWF0dGhldy5ldS5hdXRoMC5jb20vIiwic3ViIjoiYXV0aDB8NWU0N2VmYzc2N2YxYmEwZWJiNDIwMTYzIiwiYXVkIjoiTXVzaWMiLCJpYXQiOjE1ODE4NjI0NjksImV4cCI6MTU4MTg2OTY2OSwiYXpwIjoiVGh2aG9mdmtkRTQwYlEzTkMzSzdKdFdSSzdSMzFOZDciLCJzY29wZSI6IiIsInBlcm1pc3Npb25zIjpbImNyZWF0ZTphY3RvcnMiLCJjcmVhdGU6bW92aWVzIiwiZGVsZXRlOmFjdG9ycyIsImRlbGV0ZTptb3ZpZXMiLCJlZGl0OmFjdG9ycyIsImVkaXQ6bW92aWVzIiwicmVhZDphY3RvcnMiLCJyZWFkOm1vdmllcyJdfQ.iScamWOFNx9pjiVZhsvPzDoRi6EraZaxWg-WMj80HNW_-dchkOymnKA7OOhPQ8svLc9-wViLlCT-ySnupZ-209cIBVHSA_slncSP-lzEM6NKbBmDEETTQ1oxv2jTH-JL72eLhyAWUsmSIZDmEab1hln1yWEN7mUnn0nZJfxCRCs89h5EGJzXS2v8PbAjq9Mu7wFsrioEMx_PGWzSM0r5WIrKBvpXRy0Jm-vssZl4M1akDHIL5Shcfp_Bfnarc2OLOMvdQVHVDEWhrbFSnfCENLDxkcmB18VnOedJAuY_C88YRUfY2wQAOPux8RVuqIb5KxTg4YP7kiDcBUKXEnhL9A"
}
```