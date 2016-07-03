# ASP.NET MVC
- Separation of Concerns is one of the primary reasons why ASP.NET MVC exists because its very nature separates the presentation and business layers. 
- ASP.NET = Active Server Pages, .NET is not an acronym. 

## Model
- Handles Business logic.
- Manages data access and performs the business logic on the data.
- **Domain Model approach:**
  - When the object you are using describes the data you work with in the middle tier of that application. i.e. passing Entity framework objects to a view for display. 
- **View Model approach:**
  - Describes the data being worked on in the presentation layer.
  - Any data you present in the view is found within the properties of the view model class, which represents all the data the controller transmits to the view after processing the request. 
  - A view model is generally the result of aggregating multiple classes into a single object.
- **The Input Model approach:**
  - Represents the data being uploaded to the server from the client with each individual HTTP request. 
  - Uses model binding to capture user input.
  - Use of an input model enables all the work to create and manage these domain objects to stay within a single controller and model.  
- Model binders are a way to map posted form data to a type and pass that type to an action method as a parameter.

## Controllers
- Handles incoming requests, user input and interaction, and executes application logic.
- Calls the model to get required business objects and then calls the view (with or without model) to render HTML.
- Responsible for:
  - locating appropriate action method to call
  - validating that the action can be called
  - getting values in the model to use as parameters
  - managing all errors 
  - and calling view engine to write the page.
- **ASP.NET MVC apps are organized around controllers and action methods.**
- Action methods are typically 1-1 mappings to user interactions. Each user interaction calls a URL which finds the method in a controller etc.
- An action method is called everytime a user does something that interacts with the server.
- Controller actions have attributes that provide additional info to the framework:
  - most used select attributes are ActionName, AcceptVerbs, and NonAction (help determine which action to run)
  - Filter attributes enable caching, validation, error handling through use of OutputCache, ValidateInput, and HandleError. 
  - It's possible to create custom action filters with custom logic by overriding the base ActionFilter class.

## Asynchronous Controllers (Can help with performance / multi-threading etc)
- ASP.NET MVC 3 AsyncController needs to be implemented
- ASP.NET MVC 4 uses the concept of async controllers in the default controller class. 
- Asynch actions ASP.NET MVC 4: use the Task framework in the System.Threading.Tasks namespace. (Purpose of Task is to provide pluggable architecture to increase flexibility to make multi-tasking apps easier to write.) 
  - To create async action on controller:
    - Mark controller as async and return result as Task<ActionResult> 
- **Async action: Approach 1:**
  - You can create an action that returns synchronously but uses async work within the method to get work done faster. (The main thread has to wait only for the longest-running work unit to respond rather than waiting for all the work to occur, one after the other).
  - Makes sense if you are merging results from multiple service calls into a single model to be passed to the view.
- **Async action: Approach 2:**
  - Use async partial view 
  - This helps the overall performance of your app by running the work in that partial view in a different thread, enabling primary thread to continue to process other items.
  - Also helps avoid thread locking because your MVC4 app parses the action.
- **Async actionL Approach 3:**
  - Break content out on the page
  - load it async from the client.
  - i.e. Create your page normally, but rather than directly calling the action result in your .cshtml file, instead use JS code that calls the server to ask for the partial view result after the page has been rendered on the client side, a traditional AJAX approach. 
- Asynch action methods are useful for:
  - long-running, non-CPU-bound requests since they avoid blocking the server from performing work while the method request is still pending.
- **Strongly consider asynch methods when**:
  - The operation is network-bound or I/O rather than CPU-bound. 
  - Also async methods make sense when you want to enable the user to cancel a long-running method.
- Async programming gives you different ways to solve performance issues where multi-threading might help.

## Actions 
- An action can return a (result):
  - View
  - Partial View
  - JSON 
  - Binary Data
  - or Redirect to another action
- Action names are important as the name is part of the URL request.
  - Should be short and descriptive (do not provide too much of the business process in the name for security reasons)
  - Actions that do the same thing to different objects (across different controllers) should have the same name. 
  - By convention, do not contain name of the controller in the action name.

## Routes and Routing
- routing table stored in Global.asax file.
- helps URL mapping routes to handle mapping to controller and actions. 
- ASP.NET has some default routing:
  - default routing format is {controller}/{action}/{id}

## Views
- Responsible for displaying information to users (Only part users see). 
- A set of messages is sent to the view via viewDataDictionary, which is wrapped by a ViewBag. 
  - Can set and read values like a standard dictionary: ViewData["UserName"] = User.UserName
  - Can also access the data in the ViewBag as a wrapper: ViewBag.UserName = User.UserName
- The useo f inline code in the view should be strictly limited to those items that affect only the display of information, not the processing of information.
- Additional Considerations when working with a view:
  - **Strongly-typed views**
    - Eliminates need for casting in the view. 
    - View engine can work with info through mapped class values rather than through string-based lookup.
  - **View-specific model** (View Model)
    - Intermediate class for when display does not map directly to a domain object
    - View specific model gathers all values that are needed for the view from one or more model objects into a single class specifically designed for that view. 
  - **Partial View**
    - Can be displayed within a page
    - Razor view engine displays it the same as a full view, but without including the <html> tag and <head> tags.
  - **Master or layout page**
    - To share a design across multiple pages
    - Building block for app because it contains much of the wrapper HTML code that turns your output into a format understood by web browsers.
  - **Scaffold template**
    - Creates standard pages as part of the process when creating a project
    - Quick start on development, can alter existing scaffold types or create a new one.

## Razor View and Web Forms View Engines
- Razor view engine is default view engine in ASP.NET MVC4 and was introduced in ASP.NET 3.
- Web Forms Engine was the initial view engine.
- Razor view provides a format that minimizes the amount of coding required within a view and supports concepts of layouts. 
- The Razor engine uses the @ code delimiter.
- The Web Forms view engine uses the <% notation.
- The different view engines cannot process each other's syntax. 
- Blocks of code are pieces of code executed within the view. Avoid doing work that should properly be done in the controller or model.
- **Extending the View Engines: **
  - Can override either view engine classes to deviate from convention-based design the standard view engines must follow. 
  - Can write HTML helpers to generate HTML inside views. (HTML helpers help generate HTML controls programmatically)
  - A helper generates HTML and returns the result as a string for inclusion in the response stream. 
  - Can create HTML and AJAX-HTML for inclusion in your view, or URL helpers, which help determine the route or URL that can be accessed from both the view and controller.
  - Can write Razor helper using Razor syntax (one of the unique features of Razor)
    - They encapsulate blocks of HTML and server-side logic into reusable page-level methods.

## Entity Framework
- ORM
- Relies on DBContext class which is an abstraction over the database:
  - Manages data querying 
  - Unit-of-work approach that groups changes and persists them back to the datastore in a single transaction
  - Relies on flags to determine best way to persist data. 
- **Database First Approach**
  - Enables you to leverage an existing database schema to create entities.
- **Code First**
  - Intended to be used in scenarios in which you are creating a new database schema as part of your project
  - Enables developers to create the object structure first and then use it to create the database schema
- **Model First**
  - Intended to be used in scenarios in which you are creating a new database schema as part of your project
  - Enables designers to work in a tool that enables them to build the object model visually and will use that output to create the database schema. (Edmx Designer)
- The stateless nature of ASP.NET MVC disables some of the built-in features of Entity Framework. 
  - Have to write additional code to make the best use of the DBContext class and its approach to data access.
  - Best to abstract the data access layer using the Repository pattern. 

## Data Access Options
- **Option 1: ORM**
  - Aids in conversion of data within a relational database management system and the object model that is necessary for use within OOP. 
  - ORM's in ASP.NET MVC 4: NHibernate, Entity Framework, Linq-to-SQL. 
- **Option 2: Writing your own component to manage interactions with the database**
  - Manage any conversions to and from your object model
  - Preferred when working with a data model that does not closely model your object model, or you are using a database format that is not purely relational i.e NoSQL. 

## Integrating Web Services
- Standard for putting services into the app space is Microsoft Windows Communication Foundation (WCF).
  - WCF is a SOAP-based protocol and is still the primary communications mechanism
  - Completely replaces the old ASMX.
  - But ASP.NET MVC 4 Web API has made advances in RESTful services.
- With ASP.NET MVC 4, concept of WEB API was introduced:
  - Allows you to bind data using model binding directly to the output
  - Can also use ASP.NET 4 to create REST services.
  - The ASP.NET Web API comes with its own controller called ApiController for REST since it returns serialized data. 
    - It does not use views; instead it reviews the HTML header to find Accepts property being sent with the header to determine how to send data back. 
    - Do not have to handle serialization and deserialization, the ApiController handles it for you.
  - Also uses the ASP.NET MVC pattern for managing HTTP requests.
- ASP.NET Web Services (ASMX) is older and enables devs to quickly roll out a SOAP-based web service. 
  - Use a WSDL to communicate with consumers about endpoints, protocols, and message formats.
  - Eliminates many configuration issues you encounter with other solutions because it simply enables a consumer to make a call to a function. 
  - However, cannot customize certain components such as transfer protocols, security and encoders.
  - ASMX has been superseded by WCF and Web API. 

## Designing a Hybrid Application
- A hybrid application is an application hosted in multiple places. 
  - The term became popular with growth of Windows Azure to represent an app in which one part is hosted within the company's network and another part is hsoted in Windows Azure. (To access private or sensitive data)
  - An app that is partially deployed on-premise and partly off-premise. 
  - When working in this kind of environment, you need to be aware of the riskier nature of communications and manage the concept of a retry.
- **Hybrid Pattern 1: Client-centric Pattern**
  - Client app determines where the app needs to make its service calls. 
  - Easiest to code, but most likely to fail
  - Fragile because any change to either server or client might require a change to the other part. 
- **Hybrid Pattern 2: System-Centric Approach**
  - Take a more service-oriented architecture (SOA) approach
  - Turning on-site systems into "services" that can be called from Windows Azure using the service Bus. 
  - i.e. AppFabric acts as a service bus, providing a single point of contact/service connector that can manage calls out to remote systems by routing the requests to the appropriate server.
  - Ideally includes a service bus, which will distribute service requests as appropriate whether it is to a service in the cloud, on-premise, or at anmother source completely such as a partner or provider site. 
- Concerns with distrbuted apps:
  - Connection resiliency (need to understand the concept of retry)
  - Latency and good connection properties
  - Authorization and access are complicated 
  - Must plan for consistency and concurrency
  - Must consider: Scalability, latency, cost, robustness, and security.
  
## Session Management in a distributed environment
- In ASP.NET MVC 4 you can approach sessions in two different ways:
  - **Approach 1: Use session to store small pieces of data **
  - **Approach 2: Be completely Stateless**
- Since MVC sits on top of ASP.NET, sessions are available for use throughout the app. 
- However, ASP.NET MVC 4 is designed to run in a stateless manner
- In a distributed environment, requests can be distributed among different servers when using a session. 
- There are 3 modes of session management available in IIS:
  - **InProc**
    - Is the default setting
    - Web sessions stored in web server's local memory
    - Provides best performance but is not clusterable
  - **StateServer**
    - Session information is stored in memory on a separate server
    - When configuring the state server in IIS, you need to enter the connection string to the server
    - All servers using same state server have access to the state info
  - **SQLServer**
    - Has same advantages as StateServer: session info is shared across multiple servers
    - Has performance impact: It will add latency to the session access since there needs to be a call to a SQLServer.
- A Distributed Environment can improve availability, reliability, and scalability.
  - One of the ways you can achieve that at a web server level is to use a web farm.

## Windows Azure
- Microsoft cloud computing platform used to build, deploy, and manage applications through a global network of Microsoft-managed data centers. 
- Azure is a stateless system.  
- Provides both Platform as a Service (PaaS) and Infrastructure as a Service (IaaS)
  - PaaS: delivers a computing platform, including an OS, a programming language execution environment, a database, and a web server.
  - IaaS: Offers virtual machines.
- Classified as the public cloud in Microsoft's cloud computing strategy.
- There are 3 different types of solutions: 
  - **Virtual Machines**
    - Gives you the most control over the environment
    - Good choice for dev and testing and for running off-the-sheilf apps in the cloud
  - **Web Sites**
    - Good choice for simple web hosting and for hosting and running your ASP.NET MVC 4 apps.
    - Enables scalable experience, with fast deployment and an almost immediate startup
    - Can upgrade or downgrade this solution quickly and easily as needed
  - **Cloud Services**
    - Strictly PaaS approach
- Windows Azure startup tasks are used to perform actions before a role starts.
- **Startup tasks**
  - A startup task can be run more than once. 
  - AppCmd.exe is a Windows Azure-provided tool that enables you to manage your startup tasks
  - Can use startup tasks to install any additional software or third-party tool you might need or other specific needs etc
- There are 3 types of roles in Windows Azure:
  - **Web**
    - Use this if planning to run IIS in Windows Azure
  - **Worker**
    - Use this if you are going to run middle-tier apps without IIS
  - **VM**
    - Use this if what you want to do is beyond scope of Web or Worker
    - Gives complete access to the VM itself
- After startup tasks are completed, the OnStart method is called:
  - Can override the OnStart method
  - Need to return true from the method otherwise throws error
- After the OnStart method is called, the process calls Run: 
  - Run is a void method
  - Can use override to have apps start that can run in parallel to the main app
- Upon shutdown, the process calls the OnStop method:
  - OnStop is a void method
  - Typically used to close and clean up any processes you have started in the OnStart and Run methods.
- (Go back to this section (Windows Azure), since skipped heaps)

## Choosing a State Management Mechanism
- Web Forms use ViewState which stores info on the page in a hidden form field. 
  - Ensures every post request to the server includes the view state (carrying it's state with it).
  - This was a way to get around the concept of stateless as defined in HTTP
- ASP.NET MVC embraces the nature of a stateless app
- In ASP.NET MVC 4 app, state information can be stored in the following locations:
  - **Cache**
    - Stored on the server memory pool and shared across users
    - Provides broader scope than the other management objects since the data is available to all classes within ASP.NET.
  - **Session**
    - Stored on server and unique for each user
  - **Cookies**
    - Stored on client and passed with each HTTP request to server
    - For storing small amounts of data 
    - Max is 4 KB.
    - If you want client side info to persist across different pages etc such as login credentials.
  - **QueryString**
    - Passed as part of the complete URL string
    - Has several encruption schema support
    - For storing small amounts of data and visibility isn't a concern
    - Not secure and limited in size
  - **Context.Items**
    - Part of the HttpContext and lasts only the lifetime of that request
    - Info that is only available during a single request. 
    - Typically used to add info to the request that will be available to other modules and to the handler. E.g. Authentication. 
  - **Profile**
    - Stored in a database and maintains info across multiple sessions
    - Part of the Membership and Roles provided
    - Need to configure a provider in the Web.config file
    - Need to be using the ASP.NET membership provider.

## Sessionless State
- Is a way to maintain state without supporting any session models.
- Need to determine how you will pass the unique identifier from request to request.
- There are lots of mechanisms available in ASp.NET MVC 4 to help you do this:
  - Create identifier on server the first time user visits the site and continue to pass this info from request to request
  - Use hidden form fields to store and pass info from 1 request to the next.
    - There is some risk because a careless dev could forget to add the value, and you will lose your ability to maintain state.
  - Use Razor view engine to script the unique identifier storage in layout or master page, so it will render in every page.
  - Add JS functionality to store the unique identifier on the client side in a sessionStorage or localStorage and make sure you send it back to the server when needed.
  - Add the unique identifier to the query string.
  - Add the unique identifier to the URL and ensure routing tabel has the value mapped accordingly.

## Caching
- **Page output caching**
  - Shared strategy on clients and servers.
  - Can set expiration duration.
  - Types include: 
    - Full page caching
    - Partial page caching
      - **Donut Caching**
        - Caches majority of the page, enabling some dynamic content
      - **Donut Hole Caching**
        - Enables a majority of the page to be dynamic and caches some content.
- **Data Caching**
  - Server-side technique enabling you to put an intermediate step between your business logic and the database
  - Provides a way to reuse data 
  - Enhances performance by making database calls only when the cache is invalidated or expired.
  - Can set expiration period on the caching.
  - This caching is used by all users on the server.
  - Can decrease load on database and increase app responsiveness. 
  - Static queries in which the data is unlikely to cahnge often are excellent candidates for implementing data caching.
  - Best practice: Put calls to the caching service in the model since it contains primary business logic. 
- **Application caching**
  - Is an HTML5 feature that enables you to create a caching manifest that describes the settings across a website or for a page
  - Gives devs access to the local browser cache. 
- **HTTP caching**
  - Is a caching mechanism built into the HTTP protocol that handles its own version of expiration calculation and uses it to determine the response to send to the client.
  - Generally used in distrbuted systems, especially the internet. 
  - Happens automatically as part of the request stack and there is little programmatic impact.
  - HTTP server must respond to a request with the most-up-to-date response held by the cache that is equivalent to the source server.
  - Validation model: Server detects whether any changes need to be returned. Server sends a 304 (Not modified) response without a body, when there has been no change, otherwise sends full response and body.
- **Distribution Caching**
  - Work with web farms
  - Most complicated
  - Create data on one app server and share it with the other servers. 
  - Windows AppFabric is an example of a third-party tool that enables you to create caching content on one server and share it across multiple servers in a web farm.
  - (Look this up more when needed)
- (Look it up)