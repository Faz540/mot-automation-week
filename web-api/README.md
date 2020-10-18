# MOT Automation Week - Challenge: Web-API

## Task Summary:

I used Postman to create an Automated Test Framework against the [Restful Booker API](https://github.com/mwinteringham/restful-booker-platform)

The tests can be run against your local version of the Restful Booker API.

I've attached the Postman Collection and Environment JSON files which you can import into Postman.

Once you have the Restful Booker API running locally with the data initalised, you can run the entire Postman collection using the Collection Runner.

## Challenges:  

### Challenge 1: The Strengths and Weaknessess of Postman
One of the challenges I frequently face when using Postman as a test automation tool, is keeping your framework [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)

I spoke about my challenges with this at POST/CON 2019 and what steps I took to deal with this. [You can view my talk here](https://www.youtube.com/watch?v=lmGrMA4I5CI)

With Postman, I've come to accept that a certain amount of snowballing is invetible.

For example, I can't test a ```GET */user/{userId}``` endpoint in isolation as a user needs to exist/be created in order for me to test this hypotehical API endpoint. 

Outside of Postman, we would normally use BEFORE or BEFORE EACH hooks that would set up the data for the 'thing' that is under test. Hence why I would call our hypothetical ```POST */user``` API Endpoint as part of our SETUP script or BEFORE hook when testing the ```GET */user/{userId}``` API Endpoint.

Here's some fake JS code to help explain things:
```
let userId;

describe("User API Tests", function() {
    before(function() {
        userId = userApi.createUser(userDetails);
    });
    it("Expects a User to exist", fucntion() {
        userApi.getUser(userId);
    });
});
```
I don't like to rely or assume that data already exists when testing certain endpoints (Unless that data has specifically been created for testing purposes), hence why I'm setting up the data as part of my framework.

And yes, our hypothetical Create User API Endpoint will have their own 'isolated'/targeted tests. So if this endpoint was broken, it would have it's own dedicated test suite to clearly report this.

The same principle applies in Web UI Automation, if we wanted to test that an item has successfully been added to the basket/cart. Something needs to happen BEFORE visiting the basket page in order to successfully run this test, that something being 'adding an item to the user's basket/cart'

So with all that in mind, I tried to be creative and include those principles in my Postman collection inside my "BEFORE ALL:" or "AFTER ALL:" folders.

### Challenge 2: Where Do I Supply These Tokens!? Swagger Says The Body??

Another challenge I faced was my confusion around the Swagger docs, bear in mind I haven't used Swagger documentation in many years!

The main thing that confused me was supplying a token when using the DELETE and PUT API Endpoints.

Swagger would say the token would be supplied in the body on these requests which confused me. I would be supplying two seperate JSON request bodies!?

After a lot of trial and erroring I decided to look at the unit tests and I could see that the token was actually supplied as a cookie. Once I had discovered this nugget of information, I had no futher issues with this problem.

### Challenge 3: Genuine Bugs vs 'Features' 

I found a couple of scenarios that didn't work as expected.

I have marked these test failures in Postman as "KNOWN FAILURES" as in "I am aware that these tests fail, but if other tests fail then I should be worried!"

I decided to leave these failures in the framework so Mark or Richard can review and provide feedback :)
