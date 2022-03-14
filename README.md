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

![Diagram of system components](DevView.png)
This diagram represents the basic file structure of the codebase, not sure about server calls since they are locked behind Google implementations of certain imports and exports; There are many more modules under the folder, @Harper I will add those in for the final draft if there's not enough detail in the current diagram (Ryan)

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

### Bug Fix

### Feature Improvement

### Testing
