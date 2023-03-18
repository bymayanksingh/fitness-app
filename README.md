# fitness-app

Sign up & Sign in flow:

```mermaid
sequenceDiagram
    participant User
    participant Client
    participant Auth0
    participant Server
    User->>Client: Requests to sign up
    Client->>Server: Sends sign-up request with user details
    Server->>Auth0: Sends sign-up request with user details
    Auth0-->>Server: Validates user details
    Server-->>Client: Sends confirmation of successful sign-up
    Client->>User: Displays confirmation message
    User->>Client: Sends login credentials
    Client->>Server: Sends login request
    Server->>Auth0: Sends login request with credentials
    Auth0-->>Server: Validates credentials
    alt Credentials valid
        Server->>Auth0: Requests access token
        Auth0-->>Server: Generates and sends access token
        Server-->>Client: Sends access token
        Client->>User: Redirects to home page with access token
    else Credentials invalid
        Auth0-->>Server: Sends error message
        Server-->>Client: Sends error message
        Client->>User: Displays error message
    end
    User->>Client: Sends request for protected resource
    Client->>Server: Sends request with access token
    Server->>Auth0: Sends access token for validation
    Auth0-->>Server: Validates access token
    alt Access token valid
        Server-->>Client: Sends requested resource
        Client->>User: Displays resource
    else Access token invalid
        Auth0-->>Server: Sends error message
        Server-->>Client: Sends error message
        Client->>User: Displays error message
    end
    User->>Client: Logs out
    Client->>Server: Sends logout request with access token
    Server->>Auth0: Sends access token for revocation
    Auth0-->>Server: Revokes access token
```

Profile CRUD flow

```mermaid
sequenceDiagram
    participant User
    participant Client
    participant Server
    participant Database
    User->>Client: Sends request to create profile
    Client->>Server: Sends request with user information
    Server->>Database: Saves user information in profile table
    Server-->>Client: Sends success response
    alt Profile creation successful
        Client->>User: Displays success message and redirects to dashboard
    else Profile creation failed
        Server-->>Client: Sends error message
        Client->>User: Displays error message and prompts for correction
    end
    User->>Client: Requests to view profile
    Client->>Server: Sends request with user ID
    Server->>Database: Retrieves user information from profile table
    Database-->>Server: Sends user information
    Server-->>Client: Sends user information
    Client->>User: Displays user information
    User->>Client: Sends request to update profile
    Client->>Server: Sends request with updated user information
    Server->>Database: Updates user information in profile table
    Server-->>Client: Sends success response
    alt Profile update successful
        Client->>User: Displays success message and redirects to dashboard
    else Profile update failed
        Server-->>Client: Sends error message
        Client->>User: Displays error message and prompts for correction
    end
    User->>Client: Sends request to delete profile
    Client->>Server: Sends request with user ID
    Server->>Database: Deletes user information from profile table
    Server-->>Client: Sends success response
    alt Profile deletion successful
        Client->>User: Displays success message and logs out
    else Profile deletion failed
        Server-->>Client: Sends error message
        Client->>User: Displays error message and prompts for retry
    end
```

Privacy Settings
```mermaid
sequenceDiagram
    participant User
    participant Client
    participant Server
    User->>Client: Navigates to Privacy Settings page
    Client->>Server: Sends request for user's current privacy settings
    Server-->>Client: Returns current privacy settings
    Client->>User: Displays current privacy settings
    User->>Client: Makes changes to privacy settings
    Client->>Server: Sends updated privacy settings
    Server-->>Client: Validates updated privacy settings
    alt Settings valid
        Server-->>Server: Updates user's privacy settings in database
        Server-->>Client: Returns success message
        Client->>User: Displays success message
    else Settings invalid
        Server-->>Client: Returns error message
        Client->>User: Displays error message
    end
```

Profile Discovery
```mermaid
sequenceDiagram
    participant User
    participant Client
    participant Server
    participant Database
    User->>Client: Requests to discover profiles
    Client->>Server: Sends request with search parameters
    Server->>Database: Searches database for matching profiles
    Database-->>Server: Returns list of matching profiles
    Server-->>Client: Sends list of matching profiles
    Client->>User: Displays list of matching profiles
    User->>Client: Requests to view a profile
    Client->>Server: Sends request with profile ID
    Server->>Database: Searches database for profile with given ID
    Database-->>Server: Returns profile information
    Server-->>Client: Sends profile information
    Client->>User: Displays profile information
```

Profile Follow/ Unfollow 
```mermaid
sequenceDiagram
    participant User1
    participant User2
    participant Client1
    participant Client2
    participant Server
    participant Database
    User1->>Client1: Requests to follow User2
    Client1->>Server: Sends follow request with User2's ID
    Server->>Database: Adds User2's ID to User1's following list
    Database-->>Server: Confirms update to following list
    Server-->>Client1: Sends confirmation of follow
    Client1->>User1: Displays confirmation of follow
    User2->>Server: Sends notification of new follower
    Server->>Database: Adds User1's ID to User2's followers list
    Database-->>Server: Confirms update to followers list
    Server-->>Client2: Sends notification of new follower
    Client2->>User2: Displays notification of new follower
    User1->>Client1: Requests to unfollow User2
    Client1->>Server: Sends unfollow request with User2's ID
    Server->>Database: Removes User2's ID from User1's following list
    Database-->>Server: Confirms update to following list
    Server-->>Client1: Sends confirmation of unfollow
    Client1->>User1: Displays confirmation of unfollow
    User2->>Server: Sends notification of unfollower
    Server->>Database: Removes User1's ID from User2's followers list
    Database-->>Server: Confirms update to followers list
    Server-->>Client2: Sends notification of unfollower
    Client2->>User2: Displays notification of unfollower
```

User Notification

```mermaid
sequenceDiagram
    participant User
    participant Client
    participant Notification Service
    participant Server

    User->>Client: Performs an action that triggers a notification
    Client->>Server: Sends request to trigger notification
    Server->>Notification Service: Sends request to send notification
    Notification Service->>Server: Sends confirmation of request
    Server->>Client: Sends confirmation of trigger request
    Client->>User: Displays confirmation of trigger request

    loop Notification Delivery
        Notification Service->>User: Sends notification
        User->>Client: Opens notification
        Client->>Server: Sends confirmation of notification receipt
        Server->>Notification Service: Sends confirmation of notification receipt
    end
```

User Analytics

```mermaid
sequenceDiagram
    participant User
    participant Client
    participant Server
    participant Analytics
    User->>Client: Interacts with the app
    Client->>Server: Sends requests with user data
    Server->>Analytics: Sends user data for analysis
    Analytics->>Server: Analyzes user data
    Server->>Client: Sends response with analyzed data
    Client->>User: Displays analyzed data
    User->>Client: Interacts with the app
    Client->>Server: Sends requests with user data
    Server->>Analytics: Sends user data for analysis
    Analytics->>Server: Analyzes user data
    Server->>Client: Sends response with analyzed data
    Client->>User: Displays analyzed data
    loop Collecting User Data
        Server->>Analytics: Collects user data
    end
```

Workout Tracking
```mermaid
sequenceDiagram
    participant User
    participant System

    User ->> System: Log in with username and password
    System ->> User: Verifies credentials and displays dashboard
    User ->> System: Selects "Workout" tab
    System ->> User: Retrieves workout history and displays in a table
    User ->> System: Selects "New Workout" button
    System ->> User: Displays workout form with list of exercises to choose from
    User ->> System: Selects an exercise
    System ->> User: Retrieves exercise details and displays in the form
    User ->> System: Enters number of sets, reps, and weight for each set
    User ->> System: Saves workout
    System ->> User: Displays workout summary, including total time and calories burned
    User ->> System: Selects "Progress" tab
    System ->> User: Retrieves progress data and displays in a chart
    User ->> System: Selects a date range to filter progress data
    System ->> User: Updates chart with filtered data
    User ->> System: Sets a new goal for a specific exercise or overall fitness
    System ->> User: Saves goal and displays confirmation message
    User ->> System: Logs out
    System ->> User: Ends session and clears user data
```

Nutrition Tracking
```mermaid
sequenceDiagram
    participant User
    participant Client
    participant Server
    participant Database

    User->>+Client: Opens nutrition tracking feature
    Client->>+Server: Requests list of foods
    Server-->>-Client: Returns list of foods
    Client->>+User: Displays list of foods
    User->>+Client: Selects a food from the list
    Client->>+Server: Requests food details
    Server-->>-Client: Returns food details
    Client->>+User: Displays food details
    User->>+Client: Enters quantity of food consumed
    Client->>+Server: Sends food consumption data
    Server->>+Database: Saves food consumption data
    Database-->>-Server: Returns success status
    Server-->>-Client: Returns success status
    Client->>+User: Displays success message
    User->>+Client: Views nutrition summary
    Client->>+Server: Requests nutrition summary
    Server-->>-Client: Returns nutrition summary
    Client->>+User: Displays nutrition summary
    User->>+Client: Sets nutritional goals
    Client->>+Server: Sends nutritional goals
    Server->>+Database: Saves nutritional goals
    Database-->>-Server: Returns success status
    Server-->>-Client: Returns success status
    Client->>+User: Displays success message
    User->>+Client: Views progress chart
    Client->>+Server: Requests progress data
    Server-->>-Client: Returns progress data
    Client->>+User: Displays progress chart
```

Personalized Recommendation

```mermaid
sequenceDiagram
    participant User
    participant Platform
    participant RecommendationEngine
    User->>+Platform: Requests Personalized Recommendations
    Platform->>+RecommendationEngine: Sends Recommendation Request
    RecommendationEngine->>+User: Requests User Information
    User-->>-RecommendationEngine: Sends User Information
    RecommendationEngine->>+Database: Retrieves User Information
    Database-->>-RecommendationEngine: Sends User Information
    RecommendationEngine->>+MachineLearning: Analyzes User Information
    MachineLearning-->>-RecommendationEngine: Sends Analysis Results
    RecommendationEngine->>+Database: Retrieves Exercise and Meal Data
    Database-->>-RecommendationEngine: Sends Exercise and Meal Data
    RecommendationEngine->>+MachineLearning: Analyzes Exercise and Meal Data
    MachineLearning-->>-RecommendationEngine: Sends Analysis Results
    RecommendationEngine->>+Database: Retrieves Past Recommendations
    Database-->>-RecommendationEngine: Sends Past Recommendations
    RecommendationEngine->>+MachineLearning: Analyzes Past Recommendations
    MachineLearning-->>-RecommendationEngine: Sends Analysis Results
    RecommendationEngine->>+Platform: Sends Personalized Recommendations
    Platform-->>-User: Sends Personalized Recommendations
```

