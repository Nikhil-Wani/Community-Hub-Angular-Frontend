# Community Hub

### BACKEND

https://github.com/Nikhil-Wani/Community-Hub-Spring-Backend

### FRONTEND

 we will start building our Frontend application. We will use angular version – Angular 9 and Bootstrap.
 In frontend we need to build components in our application that communicates with the REST API frequently.
 In this case, having good documentation for our REST APIs is necessary. On the other hand, maintaining the documentation manually is tiresome and error-prone.

### Generating REST API Documentation using Swagger and Springfox
Swagger and Springfox makes this process of generating REST API documentation quick and painless. Using these tools, we can automate the process of documentation.

### Swagger
It is an OPEN API specification that is created as a standard to describe your REST API.As we are using Spring-boot to develop our REST API we can use a library called as Springfox to automatically create JSON Documentation.

### Configure Swagger and Springfox
Now it’s time to configure Swagger and Springfox in our project, for that we will create a configuration class called SwaggerConfiguration.
This class is marked with annotation @Configuration and @EnableSwagger2

### How Springfox works?
Springfox scans our backend application and looks for all the Controllers and related components when starting up the application and it automatically generates the documentation for our REST API. Using springfox-swagger-ui, it constructs a webpage where we can see the documentation for our REST API. Once we start the application, we can see the documentation at http://localhost:8080/swagger-ui.html

### Inside Signup Component
•	Now let’s add enable routing from Signup page to Login Page on successful Registration, if the registration fails, we should see an error on the top right corner of the screen with the Toastr error message.
•	We injected the Router and ToastrService classes into our SignUpComponent.
•	Inside the signup method, if we received success response, we are using the injected router object to navigate to the Login page and notice that we are adding a query param registered: true to communicate with the LoginComponent that registration is successful.
•	If we received a failure response, we will display an error notification.

### Inside Login Component
•	Now in our LoginComponent, we inject Router and ToastrService objects, additionally we injected ActivatedRoute class to access the route parameters.
•	Inside the ngOnInit() the method, we are subscribing to the queryParams from the activatedRoute object and in case we receive a query parameter with value for registered as true, then we display the success notification – “Signup Successful” and set the value for the field registerSuccessMessage
•	Lastly, after successful Login, we are navigating to the root URL – ‘/’ and then enabling the Success Notification with the message – “Login Successful”.

### Handling Refresh Tokens
•	when our JWT is expired, our Backend application provides us a special Token called Refresh Token which can be used to request new JWT’s.
•	So that means we have to intercept each HTTP request we are making to our Backend application and check whether the JWT is expired (or) about to be expired, in that case, we will make a REST call to our backend to generate new JWT and set the Authorization Header with Bearer Scheme.
•	First we will create an interceptor in our Angular application called TokenInterceptor
•	Inside the intercept method, we receive the JWT through authService.getJwtToken(), if the Token is valid, we add the token to the Authorization Header which contains a value according to the Bearer scheme.
•	If the token is invalid, due to a 401 error, then we will request a new JWT by calling the authService.refreshToken().
•	Once the refresh is completed, we release the requests again, we will accomplish this using a BehaviorSubject

### Refresh Token Process – Step by Step
Let’s see in-details what is going in the TokenInterceptor class in much details:
•	If the token refresh process has not already started, then the isTokenRefreshing variable will be false by default, and a null value will be assigned to refreshTokenSubject object.
•	When the token refresh process started, then we set the isTokenRefreshing variable to true and once we receive the response we pass the JWT as the value for our refreshTokenSubject
•	Once the token refresh process is completed, we will finish the processing with next.handle
Create Home Page Component
To test our Refresh Token functionality, we need to make calls to some secured API from our Angular application, for that let’s create Home Page Component and here let’s retrieve all the Posts we already have in our database.

### Refactoring the Home Component
The home page component is a work-in-progress as highlighted within the red box, we are going to divide the elements inside our home component into multiple components, so that we can reuse them if necessary.

•	Post Title Component
•	Vote button Component
•	Sidebar Component
•	Topics Sidebar Component

### Create Topics
The first thing we are going to do is to generate components which hold the functionality to Create Posts and Topics.

Let’s see what’s going on inside this component
•	First things first, we declared a FormGroup with variable name createTopicsForm, along with the fields title and description and we are initializing this FormGroup inside the constructor.
•	We have the createTopics() method which reads the FormControl values for fields title and description and assigning it to the  TopicsModel object.
•	Next, we are calling the createTopics() method inside the TopicsService and subscribing to the response, once we receive a success response we are navigating to the route /list-Topics.


### Create Posts
•	our next task is to implement the functionality to create posts and view single post. 
•	To create a post we need some kind of editor a cool WYSIWYG Editor like   TinyMCE.
•	We declared and initialized the createPostForm variable of type FormGroup inside the ngOnInit()
•	Inside the FormGroup declaration we defined all the fields which our Form has and we also added Validators to this FormControl, which just a basic validation whether the provided text is not empty.
•	Next, we are reading all the Topics information as we have to display them in the dropdown when creating the post, after reading them from the TopicsService, we are assigning the response to a Topics variable.
•	Next, we have the createPost() method which first reads the FormControl values and creates the CreatePostPayload object.
•	Once we have the necessary data, we call the createPost() method inside the TopicsService, we subscribe to the response, and once we receive a success response we navigate to the home page, or else we throw an error.
•	Lastly, we have a discardPost() method which re-directs us also to the home page.

### Functionality to Add Comments

### Submitting the comments
•	We first declared the variable commentForm of type FormGroup and initialized it inside the constructor. There is a FormControl assigned to the FormGroup, which is initialized to an empty value and we have also defined a Validator, which makes sure that the given value is not empty.
•	Next, we declared and initialized the CommentPayload object, we will use this when making a POST call to the Comment API.
•	We declared the method postComment(), which reads the value for FormControl variable – text from the FormGroup – commentForm. We then assign the value to the text field of the CommentPayload object.
•	Then we are calling the postComment() method inside the CommentService, which returns an Observable, so we subscribe to it and when we receive a success response we are resetting the text variable to an empty value.


### Displaying Comments
After submitting the comments, we also implemented the logic to Display Comments:

•	We are calling the method getCommentsForPost() inside the constructor where we are calling the getAllCommentsForPost() from CommentService.
•	As this returns an Observable we are subscribing to it and assigning the response to the comments variable.
•	We also have the method getPostById() which are reading the post information and assigning it to the post variable

### Implement Voting Mechanism
•	Create Vote API takes a request body which contains fields postId and voteType.
•	We created a class called VotePayload which encapsulates the above-mentioned fields and for VoteType we created an enum with possible values as UPVOTE and DOWNVOTE.
•	Inside the VoteService class, we first injected the HttpClient class, and inside the vote() method which takes the VotePayload object as input, we are making a POST call to our REST API.

### Inside the VotebuttonComponent :
•	The VoteService, AuthService, PostService and ToastrService classes through the constructor.
•	We declared and initialized the VotePayload object inside the constructor.
•	Then we have the upvotePost() and downvotePost() methods, where we are setting the VoteType for the VotePayload object, and calling the vote() method inside the component.
•	This vote() method, is setting the value for the postId field, which is coming as input, and after that, we are calling the vote() method of the VoteService class, which returns an Observable.
•	We subscribe to the returned Observable, and in the case of a success response, we are updating the Vote Details, like the VoteCount and also an indication whether the post is either upVoted or downVoted by the user.

### Implementing Logout
•	we just have to delete the stored Auth and Refresh Tokens from the Local Storage.
•	After that, we will make an API call to the backend to delete the Refresh Tokens so that it won’t be possible to rotate the JWT’s.
•	The logout method inside the header.component.ts is calling the logout() method inside the AuthService.
•	Inside this method, we are making a POST call to our backend, this call returns a String as response, which we are just logging to the console

