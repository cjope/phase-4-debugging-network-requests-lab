# Putting it All Together: Client-Server Communication

## Learning Goals

- Understand how to communicate between client and server using fetch, and how
  the server will process the request based on the URL, HTTP verb, and request
  body
- Debug common problems that occur as part of the request-response cycle

## Introduction

Just like the last lesson, we've got code for a React frontend and Rails API
backend set up. This time though, it's up to you to use your debugging skills to
find and fix the errors!

To get the backend set up, run:

```console
$ bundle install
$ rails db:migrate db:seed
$ rails s
```

Then, in a new terminal, run the frontend:

```console
$ npm install --prefix client
$ npm start --prefix client
```

Confirm both applications are up and running by visiting
[`localhost:4000`](http://localhost:4000) and viewing the list of toys in your
React application.

## Deliverables

In this application, we have the following features:

- Display a list of all the toys
- Add a new toy when the toy form is submitted
- Update the number of likes for a toy
- Donate a toy to Goodwill (and delete it from our database)

The code is in place for all these features on our frontend, but there are some
problems with our API! We're able to display all the toys, but the other three
features are broken.

Use your debugging tools to find and fix these issues.

There are no tests for this lesson, so you'll need to do your debugging in the
browser and using the Rails server logs and `byebug`.

**Note**: You shouldn't need to modify any of the React code to get the
application working. You should only need to change the code for the Rails API.

As you work on debugging these issues, use the space in this README file to take
notes about your debugging process. Being a strong debugger is all about
developing a process, and it's helpful to document your steps as part of
developing your own process.

## Your Notes Here

- Add a new toy when the toy form is submitted

  - How I debugged:
    When Posting a blank form, I get a 500 error - so good news is it's the server's fault, not the user's. Bad news is I'm also the server so I still need to fix this.
    I'm also getting a Warning: "Each child in a list should have a unique key" which I usually see if a key isn't asigned to a <ul> or <ol> so I'm not sure if that's important - although it could mean that a param is not being assigned? Let's take a look see.
    I also get a helpful message saying "check the method of ToyContainer" so I'll take a look at that for some clues, but I'm not supposed to change any js, so..
    Looks fine. Rendering is doing as it should - the problem lies when the new toy is being created - an id is not assigned. listing all id's shows undefined. Let's take a look at the "create" method.
    Ah, we have a "NameError (uninitialized constant ToysController::Toys):"
    A far too familiar mistake - the Toy class has been pluralized and should be "Toy": toy = Toy.create(toy_params)
    I can now add my nameless, faceless toy, like it once, and donate it to a corporation guised as a charity. What a ride.

- Update the number of likes for a toy

  - How I debugged:
    Byebug in toys_controller update
    Everything seems to be set up correctly but it's just not doing anything. It just finds the toy by id and then updates it. It needs an increment for likes. Adding toy.likes + 1 before toy.update
    Oh. React already does this. I just need to return this in json form. math's taken care of.
    Nice. Toys are being liked.

- Donate a toy to Goodwill (and delete it from our database)

  - How I debugged:
    "ActionController::RoutingError (No route matches [DELETE] "/toys/2"):"
    Sounds like there is no "delete" route or at least not set up correctly? Check config/routes.rb
    Does it need to be called "delete"??
    Nope. Looks like it's working fine now.
