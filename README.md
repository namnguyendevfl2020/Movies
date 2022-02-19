# We love movies  
We love movies is an app created for users to review movies with their showing theaters.
You can access a working prototype of the ReactJs app here: https://namnguyen-movies-frontend.herokuapp.com 

## Table of Contents
* [User Stories](#User-Stories)
* [Screenshots](#Screenshots)
* [Functionality](#Functionality)
* [Technology Used](#Technology-Used)
* [Front-end Structure](#Front-end-Structure)
* [Back-end Structure](#Back-end-Structure)
* [API documentation](#API-Documentation)
* [Room for Improvement](#Room-For-Improvement)
* [Installation](#Installation)
* [Acknowledgements](#Acknowledgements)
* [Contact](#contact)
<!-- * [License](#license) -->

## User Stories 

### Landing Page
* As a user.
* I want to see all showing movies.
* So I can quickly chose them.

### All movies
* As a user. 
* I want to be able to view all movies with a brief description.
* So I can quickly review them.

### All theaters
* As a user.
* I want be alble to view all theaters with a list of movies.
* So I can decide where to see them.

### Movie Page 
* As user.
* I want to be able to view all information about a specific movies including a description, and all reviews.
* So I decide which movie to see.

## Screenshots

### Landing Page
![Landing page](/ReadmeImgs/Landing.png)

### All movies
![All movies](/ReadmeImgs/Movies.png)

### All theaters 
![All theaters](/ReadmeImgs/Theaters.png)

### Movie 
![Movie](/ReadmeImgs/Movie.png)

### ERD 
![ERD](/ReadmeImgs/ERD.png)

## Functionality 
The app's functionality includes:
* Every User has the ability to review alll showing movies
* Every User has the ability to review all movies
* Every User has the ability to review all theaters
* Every User has the ability to review a movie with comprehensive information

## Technology Used
* Front-End: HTML5, CSS3, JavaScript ES6, Bootstrap, React.
* Back-End: Node.js, Express.js, Knex, RESTful API Endpoints, Postgres.
* Development Environment: DBeaver, Heroku.

## Front-end Structure

  * __app.js__ 
      * __Header.js__ 
      * __MoviesList.js__ `(path="/")`
      * __DetailedMoviesList.js__ `(path="/movies")`
        * __DetailedMovie.js__ 
      * __FullMovie.js__ `(path="/movies/:movieId")`
        * __Details.js__
        * __TheaterList.js__ 
        * __ReviewList.js__ 
      * __TheaterList.js__ `(path="/theaters")`

## Back-end Structure

  * movies (database table)
      * movie_id (auto-generated)
      * title
      * runtime_in_minutes 
      * rating,
      * description,
      * img_url,

  * reviews (database table)
      * review_id (auto-generated)
      * content, 
      * score,
      * critic_id,
      * movie_id,
      * created_at,
      * updated_at

  * theaters (database table)
      * theater_id (auto-generated)
      * name, 
      * address_line_1,
      * address_line_2,
      * city,
      * state,
      * zip,
      * created_at,
      * updated_at

  * critics (database table)
      * critic_id (auto-generated)
      * preferred_name, 
      * surname,
      * organization_name,
      * created_at,
      * updated_at
  * movies_theaters (database table)
      * movie_id (foreign key from the "movies" table)
      * theater_id (foreign key from the "theaters" table), 
      * is_showing,
      * created_at,
      * updated_at

## API Documentation 
  API Documentation details:
  ### API Overview

  ```
  /api
  .
  ├── /movies
  │   └── GET
  │       ├── /
  │   └── GET
  │       ├── ?is_showing=true
  │   └── GET
  │       ├── /:movieId
  ├── /theaters
  │   └── GET
  │       ├── /
  ├── /reviews
  │   └── GET
  │       ├── /
  │   └── DELETE
  │       ├── /:reviewId
  │   └── PUT
  │       ├── /:reviewId

  ```
  ### Get all movies

  This route will return a list of all movies. Different query parameters will allow for limiting the data that is returned.

  There are two different cases to consider:

  - `GET /movies`
  - `GET /movies?is_showing=true`

  #### GET /movies

  Create a route that responds to the following request:

  ```
  GET /movies
  ```

  The response from the server should look like the following:

  ```json
  {
    "data": [
      {
        "id": 1,
        "title": "Spirited Away",
        "runtime_in_minutes": 125,
        "rating": "PG",
        "description": "Chihiro ...",
        "image_url": "https://imdb-api.com/..."
      }
      // ...
    ]
  }
  ```

  #### GET /movies?is_showing=true

  Update your route so that it responds to the following request:

  ```
  GET /movies?is_showing=true
  ```

  In the event where `is_showing=true` is provided, the route should return _only those movies where the movie is currently showing in theaters._ This means you will need to check the `movies_theaters` table.

  The response from the server should look identical to the response above _except_ that it may exclude some records.

  ### Read one movie

  This route will return a single movie by ID.

  There are four different cases to consider:

  - `GET /movies/:movieId`
  - `GET /movies/:movieId` (incorrect ID)
  - `GET /movies/:movieId/theaters`
  - `GET /movies/:movieId/reviews`

  #### GET /movies/:movieId

  Create a route that responds to the following request:

  ```
  GET /movies/:movieId
  ```

  The response from the server should look like the following.

  ```json
  {
    "data": {
      "id": 1,
      "title": "Spirited Away",
      "runtime_in_minutes": 125,
      "rating": "PG",
      "description": "Chihiro...",
      "image_url": "https://imdb-api.com/..."
    }
  }
  ```

  #### GET /movies/:movieId (incorrect ID)

  If the given ID does not match an existing movie, a response like the following should be returned:

  ```json
  {
    "error": "Movie cannot be found."
  }
  ```

  The response _must_ have `404` as the status code.

  #### GET /movies/:movieId/theaters

  Update your route so that it responds to the following request:

  ```
  GET /movies/:movieId/theaters
  ```

  This route should return all the `theaters` where the movie is playing. This means you will need to check
  the `movies_theaters` table.

  The response from the server for a request to `/movies/1/theaters` should look like the following.

  ```json
  {
    "data": [
      {
        "theater_id": 2,
        "name": "Hollywood Theatre",
        "address_line_1": "4122 NE Sandy Blvd.",
        "address_line_2": "",
        "city": "Portland",
        "state": "OR",
        "zip": "97212",
        "created_at": "2021-02-23T20:48:13.342Z",
        "updated_at": "2021-02-23T20:48:13.342Z",
        "is_showing": true,
        "movie_id": 1
      }
      // ...
    ]
  }
  ```

  #### GET /movies/:movieId/reviews

  Update your route so that it responds to the following request:

  ```
  GET /movies/:movieId/reviews
  ```

  This route should return all the `reviews` for the movie, including all the `critic` details added to a `critic` key of the review.

  The response from the server for a request to `/movies/1/reviews` should look like the following.

  ```json
  {
    "data": [
      {
        "review_id": 1,
        "content": "Lorem markdownum ...",
        "score": 3,
        "created_at": "2021-02-23T20:48:13.315Z",
        "updated_at": "2021-02-23T20:48:13.315Z",
        "critic_id": 1,
        "movie_id": 1,
        "critic": {
          "critic_id": 1,
          "preferred_name": "Chana",
          "surname": "Gibson",
          "organization_name": "Film Frenzy",
          "created_at": "2021-02-23T20:48:13.308Z",
          "updated_at": "2021-02-23T20:48:13.308Z"
        }
      }
      // ...
    ]
  }
  ```

  ### Get all theaters

  This route will return a list of all theaters. Different query parameters will allow for additional information to be included in the data that is returned.

  There is one case to consider:

  - `GET movies?is_showing=true`

  #### GET /theaters

  Create a route that responds to the following request:

  ```
  GET /theaters
  ```

  This route should return all the `theaters` and, the movies playing at each theatre added to the `movies` key. This means you will need to check the `movies_theaters` table.

  The response from the server should look like the following.

  ```json
  {
    "data": [
      {
        "theater_id": 1,
        "name": "Regal City Center",
        "address_line_1": "801 C St.",
        "address_line_2": "",
        "city": "Vancouver",
        "state": "WA",
        "zip": "98660",
        "created_at": "2021-02-23T20:48:13.335Z",
        "updated_at": "2021-02-23T20:48:13.335Z",
        "movies": [
          {
            "movie_id": 1,
            "title": "Spirited Away",
            "runtime_in_minutes": 125,
            "rating": "PG",
            "description": "Chihiro...",
            "image_url": "https://imdb-api.com...",
            "created_at": "2021-02-23T20:48:13.342Z",
            "updated_at": "2021-02-23T20:48:13.342Z",
            "is_showing": false,
            "theater_id": 1
          }
          // ...
        ]
      }
      // ...
    ]
  }
  ```
  ### Update review

  This route will allow you to partially or fully update a review. If the ID is incorrect, a `404` will be returned.

  #### UPDATE /reviews/:reviewId

  Create a route that responds to the following request:

  ```
  PUT /reviews/:reviewId
  ```

  A body like the following should be passed along with the request:

  ```json
  {
    "score": 3,
    "content": "New content..."
  }
  ```

  The response should include the entire review record with the newly patched content, and the critic information set to the `critic` property.

  ```json
  {
    "data": {
      "review_id": 1,
      "content": "New content...",
      "score": 3,
      "created_at": "2021-02-23T20:48:13.315Z",
      "updated_at": "2021-02-23T20:48:13.315Z",
      "critic_id": 1,
      "movie_id": 1,
      "critic": {
        "critic_id": 1,
        "preferred_name": "Chana",
        "surname": "Gibson",
        "organization_name": "Film Frenzy",
        "created_at": "2021-02-23T20:48:13.308Z",
        "updated_at": "2021-02-23T20:48:13.308Z"
      }
    }
  }
  ```

  #### UPDATE /reviews/:reviewId (incorrect ID)

  If the given ID does not match an existing review, a response like the following should be returned:

  ```json
  {
    "error": "Review cannot be found."
  }
  ```

  The response _must_ have `404` as the status code response.

  ### Destroy review

  This route will delete a review by ID. If the ID is incorrect, a `404` will be returned.

  #### DELETE /reviews/:reviewId

  Create a route that responds to the following request:

  ```
  DELETE /reviews/:reviewId
  ```

  The server should respond with `204 No Content`.

  #### DELETE /reviews/:reviewId (incorrect ID)

  If the given ID does not match an existing review, a response like the following should be returned:

  ```json
  {
    "error": "Review cannot be found."
  }
  ```

  The response _must_ have `404` as the status code response.

## Installation

1. Fork and clone this respository.
1. Run `npm run env`.
1. Update the `.env` file with the connection URL's.
1. Run `npm run install` to install project dependencies.
1. Run `npm run start` to start your app in development mode.

If you have trouble getting the server to run, reach out for assistance.


## Acknowledgements

- This project was inspired by Thinkful We-love-movies capstone.
- Many thanks to [Thinkful](https://www.thinkful.com/), and [Stackoverflow](https://stackoverflow.com/).

## Contact
Created by [Nam Nguyen](https://namnguyen-portfolio.vercel.app) - feel free to contact me!

