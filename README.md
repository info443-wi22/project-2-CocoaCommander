# FirebaseUI Web

[FirebaseUI Web](#firebaseui-web)
  - [Report Overview](#report-overview)
  - [What is the software system, and what does it do?](#what-is-the-software-system-and-what-does-it-do)
    - [About](#about)
  - [Who created the software, and who currently "maintains" it?](#who-created-the-software-and-who-currently-maintains-it)

## Report Overview

### What is the software system, and what does it do?

Taken from the main repository: [FirebaseUI Web](https://github.com/firebase/firebaseui-web)

### About
FirebaseUI is an open-source JavaScript library for Web that provides simple, customizable UI bindings on top of Firebase SDKs to eliminate boilerplate code and promote best practices.

*FirebaseUI Auth provides a drop-in auth solution that handles the UI flows for signing in users with email addresses and passwords, and Identity Provider Sign In using Google, Facebook and others. It is built on top of Firebase Auth.*

In simpler terms, FirebaseUI helps allow componts to be created within a web-app. On the otherhand FIrebaseUI auth will also allow for users to be able to login into a web-app using their Google account, not having to create an entirely new account for a specific web-app.

### Who created the software, and who currently "maintains" it?

FirebaseUI Web is an extension / addition to Firebase that was created by Google. As this is an open source codebase, anybody can contribute to it. With that being said, all submissions that are made, even those that are project members, require review. They use GitHub pull requests for this purpose.

## Development View
### System Diagram
![Diagram of system components](DevView.png)
This diagram represents the basic file structure of the codebase, which is also how the processes are generally structured.

#### System Components/Source Code Structure
| Component                    | Description                                                                                                                                                         |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| firebaseui-web               | Main folder for modules. Imports google closure library for `goog` namespace (general javascript library for fast compilation of commonly used functions + testing) |
| externs                      | namespace for closure compiler to access + minify + speed up js compilation                                                                                         |
| data                         | module specifically for fast country lookup by phone number                                                                                                         |
| data/country.js              | implements prefix trie for previously stated purpose                                                                                                                |
| ui                           | folder for ui components                                                                                                                                            |
| ui/mdl.js                    | global style and interactions for ui components                                                                                                                     |
| ui/page                      | all components related to page functionality                                                                                                                        |
| ui/element                   | all components related to element functionality, deals with binding elements and their handlers                                                                     |
| widgets                      | core components and functionality                                                                                                                                   |
| widgets/handler              | event handlers for widgets                                                                                                                                          |
| widgets/firebaseuihandler.js | The authentication handler that implements the interface used for IAP integration                                                                                   |
| widgets/authui.js            | FirebaseUI App Builder                                                                                                                                              |
| utils                        | common helper functions                                                                                                                                             |
| testing                      | Testing package for all components, individual testing files are exported from their respective folders and imported here                                           |



### Testing Strategy
Each module is paired with a testing file under the moniker of `[module name]_test.js` or similar. It appears that all these tests are imported into the testing folder, where they are tested as aggregates of the main component that they are a part of. Running `npm run test` works for testing.

## Applied Perspective

### Introduce perspective

The architectural perspective that we have chosen to review for this system is security. In Software Systems Architecture by Nick Rozanski and Eoin Woods, security is defined as the set of processes and technologies that allow the owners of resources in the systems to reliably control who can access which resources. When talking about “who” is the audience in this particular perspective, they identify it as people and parts of software that form personas that have a sense of security identity (referred to as principals). In regard to what is meant by “resources,” they mean the different sensitive parts of the system such as operations, data elements, and subsystems. Finally, regarding “access,” it refers to the actions or operations that the principals want to successfully perform which includes reading the resources, changing the resources, and executing those resources; at the same time, there has to be limited access for the principals.

### System concerns
When considering how this perspective is applicable to our system, we think it fits because the repository highlights features of user authentication. It includes features such as interaction with identity providers like Google/Facebook, authentication based on phone numbers, sign up/in with email accounts, resetting passwords, account duplication prevention, one-tap signup integration, and signing up as a guest. This is a collection of architectural activities which are used to ensure quality for the system’s views. Authenticating users of a system is a very important tactic that is used to help achieve excellence.

We consider some of the biggest concerns regarding the security perspective include confidentiality, detection and recovery, and security mechanisms. Confidentially concerns address keeping information from being viewable to anyone who doesn’t have the right to see that certain information. With this user authentication system, they directly address and try to solve the concern of confidentiality. Detection and recovery involve the system’s ability to detect breaches and recover from those break-ins; this system detects whether particular information (like an email) is registered twice. And finally, security mechanisms, are the procedures, configuration settings, and technologies required to enforce the rules established by the security policy and provide the confidentiality, integrity, accountability, and availability guarantees required by the system. Examples that can be seen in this system are the use of user name and password authentication and single-sign-on systems like using Facebook and Google accounts.

### Activities
#### 1. Identify Threats to the System
In this activity, we create an "attack tree" which represents the threat model for this system. This activity helps identify a clear definition of what needs to be protected and what it needs to be protected from. We've identified that the most realistic attack threat for this system is extracting account details from the users of this system. The general threat action for all levels of attacks is social engineering, which is the act of manipulating a person to perform actions or reveal confidential details. All in all, access to account details makes it the root of our attack tree, which is as follows:

**Goal**: Obtain user account details to bypass the user interface
1. Extract information from the database itself

1.1 Access the firebase database directly by guessing/cracking user passwords or security passwords that allow the attacker to bypass their firebase login account.

1.2 Access firebase login from someone who is an administrator of the database by bribing them or tricking them to reveal those details.

2. Extract information from the user interface

2.1 Track a user's computer keys to reveal their login.

2.2 Send a spam email to a user to track their computer screen and activity.

2.3 Create a fake website that looks like the UI and send the URL to users to trick them into giving their login.

3. Extract information from outside the firebase and UI login interface

3.1 Call/email a staff member of the system, or meet them in person, to trick them to reveal details.

3.2 Get access to the administrator's or staff's computer to either find login details or automatically bypass the UI through the "saved passwords" feature on browsers.

3.3 Hack the Google, Facebook, Twitter, or Github database if the user logs in through those avenues.

#### 2. Assess the Risks
Now, we reevaluate the likelihood of these attacks occurring and whether it is an acceptable level of security risk for this system. To reiterate, we believe that the most common attack to reach the main goal in our attack tree is social engineering. Depending on the administrators of the system and its user account information, it would be hard to assess how likely it is that an attacker can easily go in and talk their way through getting sensitive information. Maybe an attacker targets an employee/administrator and invites them out for drinks or develops a close relationship that allows them access to office materials. The possibilities are endless depending on how big the organization is and how valuable its user information is. Therefore, for the first two actions on the attack tree, we think that these are more likely to occur in the system's lifetime.

For the last action we identified in the attack tree, we think this is the least likely scenario to happen. Although social engineering can theoretically penetrate any security system, any organization that uses this system and has security protocols for their information would be most protected from this attack. Non-disclosure and confidentiality agreements can hold employees and administrators accountable for the information they leak. Also, using SSO for any system such as Google, Facebook, or Twitter makes it more difficult to hack because their security system is more robust. Any effort from an attacker to divulge the system's user information outside of the organization's database and the user interface is most improbable.


## Styles/Patterns Used

### Identifying Architectural Style
We would suggest that the architectural style that is found throughout this code base would have to be a Flux architectural style. Looking into the flow of information within the different components, we found that there is a unidirectional flow of information, as it will flow in one direction. The components of the codebase build off of eachother but will only be taking in the necessary parameters to complete the function that they are asked to complete. As this will be dealing with user authentication, there will be containment implemented to add a level of security to the data being used. 

### Design Pattern and Explanation 
- Adapter intent:
    - **FirebaseUiHandler**: takes a configuration object that specifies the tenants and providers for user authentication 
- Proxy Pattern:
    - Found in the API reference of the FirebaseUIHandler 
- Composite Pattern:
    - Found when configuring sign-in providers
- Strategy Pattern:
    - Found within phoneconfirmationcodetesthelper.js

## Architectural Assessment

### Single Responsibility Principle
- Every class only has one responsibility.
  - Each module is separated from each other and have one purpose; UI components, logic functions, authentication and database screens, etc
  - Modules are separated in different files with different names and are grouped together based on their type of purpose(“testing” folder have files that test the code)
### Open-closed principle
- Software entities are open for extension but closed for modification
    - Allows users to use the class for firebase authentication while allowing users to customize its UI
    - The initial code should not be modified but could be utilized or extended for other purposes that fulfills something else
### Interface Segregation Principle
- Different client-specific interfaces, rather than one general purpose interface
    - The classes do not depend on things that they don’t need; only takes in needed parameter to run the code
    - Does not contain unnecessary code or redundancy

## System Improvement

### Refactoring
1. The `tearDown()` function is defined multiple times with both the same and different definitions.
    - `tearDown()` under the `page` folder only refers to one implementation. Additionally, all tests under the page folder have a dependency on the `pageTestHelper` module, so `tearDown` will be extracted to that place.
2. Testing modules are in the same folder as the modules that they are testing
    - These should be moved to a separate testing folder so that developers will have an easier time finding the main code file they want to edit rather than opening up both the test file and the non-test file if they are unable to read the full filename.
3. `dialog.js`'s `showDialog()` function is extremely long with in-line comments describing most of the functionality
    - A lot of this functionality can be extracted into helper functions that are more descriptibe of what the code actually does.
4.
5.
6.
7.

### Bug Fix
We also took on issue [issue #702](https://github.com/firebase/firebaseui-web/issues/702), which is labeled as 'internal-bug-filed' and 'type: feature request'. This issue addresses the problem of the inability to sign in with a LinkedIn account. The problem with this addition of authentication is that Firebase only supports four federated Identity Providers: Google, Facebook, Twitter, and GitHub, which makes it difficult to incorporate other platforms that users can sign on to and create an account.

We found a [stackoverflow post](https://stackoverflow.com/questions/40040025/has-someone-managed-to-implement-a-linkedin-login-with-firebase-on-ios) where someone asks if anyone has managed to implement a LinkedIn login feature with Firebase. The top answer led us to an [article](https://firebase.googleblog.com/2016/10/authenticate-your-firebase-users-with.html) which gives steps on how one might do this.

Although it was difficult to accurately pinpoint how we can apply this to our system, we still attempted it. First, we found the files where the system initializes other login providers, which was in app.js and widget.html in the demo/public folder. Both of these files show to be the beginning construction in the user interface because it contains initializations of the other providers and email signup options. Then, in common.js under the same public folder contains methods that help build the login systems in the app and widget, like getEmailSignInMethod().

In this file, we added the steps that were provided in the article, though the result of these steps may not align with the other methods because it returns an HTML piece instead of a basic configuration statement like in the email method. However, publishing these steps could be helpful to future developers because the end result is with the common methods and they won't have to spend time scouring the internet for other solutions.

### Feature Improvement
The feature improvement we chose to do is [issue #908](https://github.com/firebase/firebaseui-web/issues/908). This issue addresses during account creation when using firebaseui-web, only a minimum of 6 characters with a mix of letters and numbers are needed to create a password. However, the requirements are too simple and would not be able to safely secure the user's account. Thus, the person who proposed this issue wanted to be able to change the minimum password requirements to at least 14 characters or to any amount.

Though it was tricky as to find exactly where the validation of the password is at, the organization of folders in the firebaseui-react is really clear. We are able to pinpoint where the problem might be located at, which is the validation of the password field. This is located in the javascript folder, then the ui folder, under the element folder and is called 'newPassword.js'. It would be in the newPassword file because it's where the password is created as and it's when the system determines if the password fits the criteria or not.

Since this is an element that have its own set of rules that's declared on that javascript file, it's where we made the changes and added another additional rule that validates it. If the length of the password input is 14 characters or more, the password is accepted and no exception is thrown. That constant length value could also be changed by the developers if they don't want that specific number.

### Testing
Each component in FirebaseUI-Web is paired with a testing file. In the case that we will examine for this report, common.js contains some functions that are neither mentioned nor tested for in common_test.js, namely `listenForInputEvent()`, `listenForEnterEvent()`, `listenForFocusInEvent()`, `listenForFocusOutEvent()`, and `listenForActionEvent()`. Perhaps these functions were tested from a different library since there are `@template` tags in the function comments, which lead me to believe that these functions were copy pasted from somewhere else, but that also begs the question, "if these functions were taken from somewhere else, why not just import them and provide more abstraction?" To that, I have no idea.

Note: theoretically, this is how testing should work, but due to a slew of deprecated packages that I can not figure out, we have elected to avoid implementing additonal unit tests as there is no way to validate any additional tests.
