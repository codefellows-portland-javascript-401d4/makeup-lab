# Make Up FullStack Lab

Create an complete fullstack lab using Node, Express, Mongo(oose), and Angular.

This app tracks restaurants and user ratings by Portland neighborhood

## App

### Main App States ("pages") aka Routes 

1. `/welcome` - A main welcome/landing page that greets users and is publically assessable.
2. `/neighborhoods` - A Neighborhood page that lists all neighborhoods showing the name of the neigborhood
along with count of saved restaurants for each neighborhood.
  1. Provide a way for the user can select one quadrant and list will filter client side to that quadrant. 
  1. User should be able to add a new neighborhood. Can be inline or use a popup.
  1. User can click on a neighborhood and transition to the `/neighborhoods/:id` state
3. `/neighborhoods/:id` - Shows a list of all restaurants that have been saved to that neigborhood, 
and an average user rating.
  1. The list should default from highest-ranking to lowest, but offer an option to switch the sort order (client side sorting).
  1. User can add a new restaurant. Can be inline or use a popup.
  1. If the user added the restaurant, they can delete it.
  1. User can click on a restaurant and go to the `/restaurants/:id` state.
4. `/restaurants/:id` - Detail view of restaurant and list of ratings by user.
  1. User can add a rating to a restaurant between 1 and 5 "starts" and leave a comment. 
  They can change their existing rating and comment, but cannot add a second.
  
Routes 2, 3 and 4 are protected, there must be a valid signed in user.

Neighborhoods, restaurants and ratings are visible across all users.

You need to offer ability to sign up and sign in.

### Testing

You need to write unit tests for all services and component controllers 
(if there _is_ js logic in them, if not don't unit test)

## Server

### API 

The server needs to offer a data API that supports the above app flows:

* Sign in, up, token validation. User needs to include a required `username` field along with `password`
* Create `neighborhood` (takes a name and "quadrant" which is one of N, NW, SW, SE, or NE. All fields required)
* Create `restaurant` (`name`, and `neighborhood` (id key to neighborhood), `createBy` (user id who created)
`address`, which is complex object with `street`, `city`, and `zip` properties. All fields required)
* Delete `restaurant` (must validate it was created by same user trying to delete) which should also delete 
any `reviews` associated with that restaurnat
* Create/Update a `review`. Review has `restaurantId`, `userId`, `rating` (number between 1 and 5 inclusive), and `comment`. 
All fields except comment are required.
Review should be updated if submited for existing user/restaurant id's.
* Return a list of `neighborhood`s with a count of restaurants.
  Hint: Here is an example of aggregation definition for grouping a count:
  ```
  Restaurant.aggegate([
    {$group: {
      _id: "neighborhoodId", // where neighborhoodId is the name of the parent (foriegn) key field
      count: {$sum: 1}
    }}
  ])
  .then(/*...*/)
  ```
  You will need to manually join the `Neighborhood.query` results with the above results.
 * Return a list of restaurants for a particular neighborhood. Should include name and average rating.
  Hint: Here is an example of aggregation definition for grouping a count:
  ```
  Rating.aggegate([
    {$group: {
      _id: "restaurantId", // where restaurantId is the name of the parent (foriegn) key field
      rating: {$avg: "rating"} // where rating is name of field that holds the 1-5 value.
    }}
  ])
  .then(/*...*/)
  ```
 * Return a particular restaurant. Includes all fields for that restuarant, plus a ratings array that 
 contains all fields for all rating records that belong to the restaurant. The ratings records should 
 populate the userName from the userId
 
 ### Testing
 
 Write E2E test for all major operations. You only need to do one golden path test for each operation, 
 not exhaustive edge cases.
 
 ## Quality
 
 * Code needs to be clean and consistent. Refactor, code and naming need to be consciously 
 improved beyond "it works" (code is read far more often than written)
 * If there are general things like linting, builds scripts, etc. that we consistently did across 
 projects, do that for this project as well
 
 ## Questions, Clarifications, Did I miss something?
 
 Post an issue to this repo.
  
