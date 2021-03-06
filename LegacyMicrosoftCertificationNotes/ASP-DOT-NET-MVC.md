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
- Session management is a state management mechanism built into Microsoft Internet Information Services (IIS). 
  - Highly configureable
  - Can set session management at the IIS level across all apps or just a single website. 
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

## WebSocket Strategy
- **HTTP Polling**
  - JS methodology of continuously polling the server to see whether there is any info the client needs to know.
  - Not the most efficient method
  - Worked in any browser supporting JS and does not require HTML5
- **HTTP long polling**
  - Way to use HTTP to mock up a way for the server to pass data back to the client. 
  - Opens a long-standing connection to the server that will either timeout or return data when the sever deems necessary. 
  - Upon timeout or data return, the client can immediately open a new connection.
- **WebSockets**
  - Way to provide 2-way communication between the client and server
  - Both sides can communicate at the same time to the other side
  - Client connects via HTTP and then sends an upgrade request to the server, which gives a WebSockets connection
  - Need to create both client-side and server-side code to interact with the socket
  - After that, every event is an event that is fired when messages are received
  - Can be used in situations in which long-term, 2 way communication is useful. 
  - Not always the best solution, especially when there is a chance the app will be viewed by older browsers that do not support HTML5 features. 

## HTTP modules and handlers (Interceptors etc)
- Enable an ASp.NET MVC 4 dev to interact directly with HTTP requests. 
  - When a request starts into the pipeline, it gets processed by multiple HTTP modules, and then processed by a single HTTP handler, before flowing back through the request stack to again be processed by the modules.
- HTTP modules and handlers insert into the request processing path in IIS. (Similar to interceptors for requests in AngularJS)
- **Modules are called before and after the handler executes. **
  - Modules fit into the process on the way down to the handler, and on the way back out from the handler. 
  - Intended to enable a developer to intercept, participate, or modify each request.
  - A Synchronous module has an Init method enabling you to set a handler for 1 of the events attached to the request process
  - An Asynchronous module is more complicated to work with 
    - But with async, await, and Task you can create an HTTP module that can handle long-running tasks without stopping the process.
- **Handlers are the destination of the requestion process and serve requests for a particular URL/Extension. **
  - Http handlers help you inject preprocessing logic based on the extension of the file name requested.
  - A handler can be synchronous or asynchronous, depending on the class they extend
- **Choosing which 1 to create is a matter of determining where in the request process you need to add your functionality**
  - If your requirements expect you to handle a specific URL or extension differently from others, a handler is prob what you need to create. 
  - If you instead, want to act when something happens during the process, you should use a module.  

## UI Design
- HTML provides structure to rendered page
- CSS provides additional control over the look and feel, or presention of the page.
- Dynamic page content is the main reason to use ASP.NET MVC 4. 
  - Dynamic content is different info dispalyed based on a set of conditions
- Razor view engine uses the _Layout.cshtml file for the primary design template for the app
  - One of the key features is the link to the CSS file that defines the styles for the site
  - This element also contains common UI elements such as menus, headers, and footers for the pages in the site. 
  - A site can have one or more CSS files.
- ASPX view engine uses the Site.Master file rather than the _Layout.cshtml file
- Helpers are ASP.NET MVC code constructs taht output HTML
  - Many different helpers such as @Html.TextBox
  - @Html.TextBoxFor links to a model.

## Validation
- **Model (MVC)**
  - Manages behaviour and data of an app domain.
  - The model validates fields required in the MVC web app
    - Validated through data annotation [Required], [Range(1, 250)] etc
      - min length, max length, data type, rules for the attribute etc
- **Views (MVC)**
  - Manages the dispaly of info 
  - Using model-based data annotations provides a view that enables you to allow client-side validation 
    - Two of the key constructs are @Html.EditorFor and @Html.ValidationMessageFor
      - EditorFor helper relates validation information in the model to the text box that displays in the editor. This relation occurs serverside where it is tied back to the model.
        - Typically, whenever you see an EditorFor, you will find an Html.LabelFor, which provides the display of the text label for the related item. EditorFor is like a smarter TextBoxFor, it figures out the type of the model and produces a textbox with that type. You can also override it so it gives default tempaltes (Look this stuff up).
        - ValidationMessageFor helper displayers validation info for the related model/input item.
- **Controllers (MVC)**
  - When using model-based data annotations, perform a check on the server side.
  - The ModelState property on the base controller has several model-specific helpers
    - IsValid property
      - Provides a list of valid and invalid fields you can use 
      - i.e. if (ModelState.Isvalid) { Save etc... }
- **Remote Validation**
  - i.e. Checking if username is taken or not. (This must be done via a remote validator to the server)
  - Remote validation has 2 parts:
    - A server action that evaluates validity
      - Typically create a validation-specific controller to handle all your validation.
      - To ensure the UI can call the validation action, add System.Web.Mvc.Remote Attribute to the validation configured on the model. 
        - [Remote("IsUserAvailable", "Validation")]
      - Also need to make configuration changes so the server knows to allow remote validation. (In Web.config):
        - In <appSettings> add the following: 
        - <add key="ClientValidationEnabled" value="true"/> 
        - <add key="UnobtrusiveJavaScriptEnabled" value="true"/>
- **jQuery UI**
  - Can help with validation like using the DatePicker to only display valid dates etc.

## Using AJAX to make partial page updates
- Can use AJAX within MVC web apps to send and retrieve data from the server asynchronously 
  - Doesn't require a entire page reload, only show and display results from the AJAX (Improves UX)
  - Use AJAX for dynamic content not static
    - If app displays info that changes frequently, use AJAX calls fired based on a timer to refresh that area on a regular basis.
  - Valid example would be a search box, making calls to the server as the user types (filtering out search results in real time).
  - System.Web.MVC.Ajax namespace helps with AJAX
  	- Has helpers and extensions
  	- Helps AJAX constructs in your view such as AJAX-based forms.
- Limitations of using AJAX:
  - Due to dnyamic nature, many different web technologies have problems understanding the info
    - Search engine web crawlers, rarely process JS, meaning the data is never indexed by a search engine. 
    - Difficult to bookmark data
    - Can be difficult for screen readers. (Hard to parse and recognize changes)
- Inserting a partial view into a different model
  - A partial view that is tied to a model might not be able to display correctly if inserted into a view being controlled by a controller other than the one that created it.
    - e.g If a partial view is created for Modle FOO, an additional workaround (such as AJAX) is required if the partial view FOO is being inserted into a view created for model BAR.

## Partial Views
- Way ot reuse functionality on multiple pages.
- Enable the developer to write code once and include it on other pages as needed.
- Are the MVC replacement for user controls from ASP.NET web forms. 
- Partial views are usually stored in the Views/Shared folder.

## Designing and implementing pages by using Razor Templates
- The Razor view engine enables you to create reusable templates.
  - The templates are assignable by object type, and can be either for dispaly or edit. 
- Razor templates are a way to create, maintain, and display sections of page layout.
  - Enable reusable code that are part of the UI layer and can be managed independently from model or controller
- ASP.NET MVC 4 has several built-in tempaltes for common classes, such as string.
- You can also create your own to display upon request by using @Html.EditorFor(model => model.Article)
  - Where article is of the type that has a custom editor template. 
- **EditorTemplate**
  - Type of template dispalyed when you use an @Html.EditorFor helper in a view
  - EditorFor tempalte is both a create and an edit template, so you must manage situations where the object passed is null.
  - When creating these templates they should be in their own .cshtml and stored in a well known directory:
    - ~/Views/ControllerName/EditorTemplates/TemplateName.cshtml
    - ~/Views/Shared/EditorTemplates/TemplateName.cshtml
- **DisplayTemplate**
  - Similar to Editor, but is just for displaying an object rather than creating or editing on the object.
  - ~/Views/ControllerName/DisplayTemplates/TemplateName.cshtml
  - ~/Views/Shared/DisplayTemplates/TemplateName.cshtml

## Designing layouts to provide visual structure
- Typically a layout for a webpage or an MVC app has a header content area, a menu area, a content area, and a footer area. 
- You can create more than one master or layout within the same folder; the layout of these other master or layout pages can be entirely different from the design of the default master or layout page.
  - Master or layout pages can be switched on the fly via code. This can be useful if the goal is to create different user experiences based on conditions.
  - Master/Layout page is used to share libraries and css files to the sub pages. 

## Detecting browser features and capabilities
- If certain feature are missing or known to behave differently than expected, they must be corrected using add-on libs such as jQuery and Modernizr.
  - The common method for browser detection is to use JS to query for the userAgent header.

## Browser compatability and dealing with different devices.
- The layout included with default MVC project tempalte supports desktop browsers running in a typical landscape view.
  - To display portrait view on desktops or to support mobile, can use @media
- Use CSS
  - Media queries
  - Use most common features availablea cross all the browsers for compatability.
  - If using specific browser methods i.e. -moz-, use fallbacks incase. 
- Use libs such as jQuery and Modernizr
  - Consistent look and feel across different browsers 
  - Accessible across all browsers and devices
  - JQuery Mobile framework provides a unifying way to manage many different mobile platforms. (Look into it.)
- Common methods for detecting the type of browser:
  - Use JS to query for the userAgent header
  - Use the DispalyMode provider.
  - The window.addEventListener method does not give any info on the browser being used by the client, but it can be used to see whether a browser is HTML5-compliant.
- DisplayModeProvider
  - If targeting smaller screens, you should create a mobile-friendly master page as well as mobile-friendly layouts and designs.
  - i.e. Different views for different browsers/devices such as Home.iemobile.cshtml and Home.IPad.cshtml
  - Add a new DisplayModeProvider for each device and create special view types you want to support
- **Summary**
  - MVC 4 supports mulitple approaches to mobile users.
    - Can create overridden views that are generic for any mobile device or specific to a device.
    - The System.Web.Mvc.VirtualPathProviderViewEngine.DisplayModeProvider evaluates incoming requests and routes them based on the values in the userAgent of the request and the configured DispalyModeProviders.
    - Another choice for mobile-viewable websites is to use the viewport <meta> tag and CSS @media queries. 
    - The jQuery Mobile lib enables you to use markup to provide additional functionality as supported by the client browser. If the browser does not support the functionality, the jQuery lib will downgrade the functionality gracefully.

## Search Engine Optimization (SEO)
- There are two primary ways to optimize your website for search indexing:
  - 1: Ensure a clear consistent message within the text on the page.
  - 2: Ensure the site is coded properly to facilitate search engine crawlers.
    - Many accessibility produces such as screen readers, depend on properly structured code to provide info to users.
- Web Accessibility Initiative-Accessible Rich Internet Applications (WAI-ARIA) is a set of descriptions on how to make active content more accessible.
  - When properly implemented, ARIA should not have any effect on search engine rankings. All markup occurs in attributes within the HTML elements.
  - Basic concept of ARIA is to provide additional context to HTML elements and to the content they contain
    - Meaning is delivered visually rather than in a format that can be referenced.
  - There are four major considations that ARIA tries to address:
    - **keyboard focus and navigation**
    - **relationships**
    - **managing dynamic changes**
    - **the role of presentation**
  - To make a fully ARIA-compliant app means you need to manage all of these aspects of your web app.
  - Limitations of ARIA 
    - Not every client supports it
    - Can provide extra text to download, sometimes doubling the payload for a server request
    - If size of payload is critical, simply adding ARIA can negatively affect performance, however if the payload is less critical, making your site accessible as possible is important.
- ASP.NET MVC 4 provides limited support for ARIA.
  - However, MVC enables you to enhance the built-in HTML helpers to provide ARIA specific information. (By extending the helpers).
- The SEO Toolkit is a widely used tool that examines HTML and reports on issues you should fix.
  - The toolkit runs under IIS.
  - You download the toolkit from Microsoft.com
  - Can analyze a website or web app, create sitemaps and create robot exclusion rules and a robots.txt file for a site, which tells search engines not to index a certain page.
-  W3C offers free validators for HTML, CSS, feed formats, and mobility
  - Can also upload rendered content while developing a site.
  - Although V3C validators do not search for SEO specifically, their evaluation of the HTML structure can improve your site's accessibilitiy for search engine crawlers and for users with disabilities.
- Another tool for validating your app is Page Inspector in Visual Studio.
  - Evalutes HTML in your app, looking for potential issues.
- **Summary**
  - SEO
    - Ensure your code does not have any missing or incorrectly ordered HTML tags
    - Ensure you have separated your content from your presentation and scripting info
      - Especially important for ASP.NET MVC apps because much of the rendered HTML is created by HTML helpers or shared from template pages, and is not usually inspected
  - HTML analysis tools can help you determine validity of the HTML your app outputs. 
    - IIS SEO Toolkit
    - Internet Explorer Developer Toolbar
    - W3C provides online validators that check HTML and CSS
  - WAI-ARIA is a markup system that helps assistive technologies, thus users, better understand and make use of your content
  - ASP.NET MVC 4 does not currently offer built-in support for ARIA. 
    - However you can extend it to create HTML helpers that extend the current set of built-in helpers

## Globalization and Localization
- Globalization 
  - **Internationalization (l18N):** 
    - Making your app able to support the use of multiple cultures 
  - **Localization**
    - Effort necessary to translate data, labels, help files, support documents, and so on to enable any user to understand the application.
    - Creating locale-specific content, images, and video - all the items your application presents to the user.
- Both l18N and localization need to be done before your app can be considered 
multicultural, and the timing of your conversion is important.
  - It can take almost as long to translate an app as it does to develop an app.
    - After you globalize an app, you don't have to repeat the effort.
  - Items that need to be translated must be pulled out of an app and put into separate resource files.
  - Your app then needs to interpret the culture in thebrowser and set the server info appropriately. 
  - Finally, after you receive the translated info, you need to make it available in your app. (Only at this point will your app be globalized)
- Look this up again if you actually want to do it.
  - Planning a localization strategy
  - Creating and applying resources to the UI
  - Settings cultures
  - Creating satellite resource assemblies
- **Summary**
  - Globalization requires you to put all displayable strings in resource files. 
     - Can choose which resource files to create
     - Should minimize duplication of strings to ensure minimal time to performing translations.
  - An alternative approach to resource files is to provide different views for different languages.
    - Eliminates dependency on resource files, but can lead to code replication.
  - Can use shared approach to globalization, in which resource files are used along with multiple copies of views.
    - Best suited to supporting non-Western languages.
    - Using another set of views strictly for right-to-left languages is a logical appraoch
  - Provide globalization resources to jQuery-specific items.
    - JS cannot determine the culture from the browser, even if the info is available and is being sent to the browser.
  - Can give users option to choose their culture in your app.

## Applying authorization attributes and global filters
- Adding attributes to a controller/action enables you to implement complex requirements across an application.
- The attribute's primary role is to analyze information, especially the HttpContext class, coming into and out of the controller to determine whether it meets a set of requirements.
  - The basic set of attributes enables a developer to put some business logic around the flow of the application.
- **Traditional filters**
  - Ensure a request meets a certain expectation:
    - RequireHttpsAttribute
      - Ensures requests go through HTTPS
    - ValidateAntiForgeryTokenAttribute
      - For protection against cross-site request forgeries
      - Ensures that there is a shared, secret value between the form data in a hidden field, a cookie, and on the server. 
      - Validates that the form is one that your server posted, that it is the same browser version, and that it matches an expected value on the server.
    - ValidateInputAttribute 
      - Gives you control over the content coming back from a post operation and ensures that there is no potentially dangerous content such as injections.
      - Can select form fields that will not be validated in the attribute by [ValidateInput(true, Exclude = "ArbitraryField")] and on a model property by AllowHtml attribute. 
    - AuthorizeAttribute
      - Gives you control over whether the user must be authenticated before being able to take the action.
      - Authorize(Roles = Admin, PowerUser)
      - In many cases, the AuthorizeAttribute is applied globally so that all actions in all controllers require that the user be authorized
      - With a few that do not require authorization being marked with AllowAnonymous.
    - and ChildActionOnlyAttribute. 
      - Looks at the application context to examine whether it should respond. 
      - Ensures that an action cannot be reached through the traditional mapping process since ChildActionOnlyAttribute can be called only from Action or RenderAction HTML extension methods such as @Html.RenderAction.
  - The remaining two filters:
    - HandleErrorAttribute
      - Error management tool handling exceptions occuring within the action
      - By default, ASP.NET MVC 4 displays the ~/Views/Shared/Error view when an error occurs in a decorated action. 
      - Can also set the ExceptionType, View, and Master properties to call different views with different master pages based on the type of exception.
      - To perform customizations or overrides using the HandleErrorAttribute, override the OnException method. 
    - ActionFilterAttribute
      - Isn't a true attribute, it's a abstract class upon which action filters are based. 
      - Enables creation of custom action filters. The 4 primary methods avaliable for override: 
        - OnActionExecuting
        - OnActionExecuted
        - OnResultExecuting
        - OnResultExecuted
- Ordering filter attributes: The first approach:
  - Put reject filters first, and in the order from the most likely to fail to the least likely to fail. 
    - This avoids extra computation since you want the app to fail faster, if there is errors.
- Ordering filter attributes: The second approach:
  - Ordering based on business importance from the most important to the least. 

## MVC Controllers and Actions 
- There are 9 default action results with ASP.NET MVC 4:
  - **ContentResult**
    - Helper method: Content
      - Returns a user-defined content type
  - **EmptyResult**
    - Helper method: (None)
      - Represents a return value to be used if the action method must return a null result
  - **FileResult**
    - Helper method: File
      - Returns binary output that is written to the result
  - **JavaScriptResult**
    - Helper method: JavaScript
      - Returns JavaScript that is executable on the client
  - **JsonResult**
    - Json
      - Returns a serialized JSON object
  - **PartialViewResult**
    - Helper method: PartialView
      - Renders a partial view
  - **RedirectResult**
    - Helper method: Redirect
      - Redirects to another action method by using its URL and passing through the routing process
  - **RedirectToRouteResult**
    - Helper method: RedirectToActionRedirectRoute
      - Redirects to another action method
  - **ViewResult**
    - Helper method: View
      - Renders a view as an HTML document
- **Summary**
  - Filter attributes provide a way for the developer to examine and take action on information in a request before and after the action is called. 
    - ASP.NET MVC comes with built-in attributes that help with authentication and authorization, secure access, anti-forgery support, and error management. 
    - You can create custom action filters as needed.
  - Action results are the finishing actions taken by an application.   
  - Model binding is a flexible process that maps fields on the UI to properties on the model. 
    - There are 3 types of model binding: 
      - **strongly-typed binding**
        - 2-way tool in that the HTML helper understands attributes on the model and can set up client-side validation based on that info. 
      - **weakly-typed binding**
        - 1-way binding in that it doesn't provide validation on the client side, but it does create the model after the request is returned. 
        - Can provide some helpers as well as create an accepted list or blocked list of attributes that should be populated from the form by using the Include and Exclude parameters on the Bind attribute.
      - **and using the value provider.**
        - Can map forms data returned from the client using the ToValueProvider on the FormCollection object. This process attempts to match the fields of an empty model with the values it finds in the list of keys.
        - If it finds a semantic match, it populates the property in the model with the value in the form collection.

## Design and implement Routes
- **Summary**
  - When SEO is important to your application, consider human-understandable URLs, such as Product/<BookTitle>, rather than something like Product/1.
  - The order you place routes into the RouteCollection object, or route table, is important. 
    - The route handler processes the list until it finds one that matches the incoming pattern
    - Should start your list with the patterns that the route handler should ignore and then add more specific URL patterns so they will be matched by the route handler before a more general one finds it. 
    - Use the MapRoute function to add a route to the RouteCollection object. 
  - When creating a route, you can add default values to take place of any values that are missing in the URL string. 
  - Constraints are a way to filter a requested URL to define different routing for items based on the variable type or content. 
    - The route handler reviews the constraint as a regular expression and evaluates the appropriate variable against it to determine whether a match exists.
  - Large, complex ASP.NET MVC apps might need to support hundreds of actions on a controller. 
    - Using **areas** enables the designer to separate functionality into logical or functional groups. 
      - Creates new copies of Models, Views, and Controllers directories in an Areas directory so you can split functionality in an appropriate way. 
      - Each has its own route management features as well, so one area can define a route different from another area. 
      - The areas are split in the application by AreaName/Controller/Action

## MVC Filters and controller factories
- Filters should be thought of as adding extra functionality consistently throughout your application. Authorization filters, for example, enable you to do custom work around authentication and authorization.
- The main filters are:
- **Authorization**
  - An authorization filter makes a security based evaluation about whether an action method should be executed, and it can perform custom or other security needs and evaluations.
- **Action**
  - Enables the developer to wrap the execution of the action method. 
  - Also enables the system to perform an additional workaround, providing extra information into the action method
  - Or it can inspect the information coming out of the action and also cancel an action methods execution.
- **Result**
  - Is a wrapper around an action result. It enables the developer to do extra processing of the results from an action method.
- **Exception**
  - Is run when there is an unhandled exception thrown in the processing of an action method. 
  - Covers the whole lifetime of the action, from the initial authorization filters through the result filter.
- When using an attribute, you can apply it as described in AttributeTargets (controller classes or actio nmethods) or you can employ it globally.
- To ensure it runs on every method, every time, add it to your global filters. 
  - You do this by adding them in App_Start/FilterConfig.cs in the RegisterGlobalFilters method:
  - filters.Add(new MyCustomAttribute());
- Custom controller factories are usually for support of DI and Inversion of control (IoC). 
  - Another reason is passing in service reference or data repository information. Basically, any time you need to create a controller in a way that is not supported by the basic functionality.
- **Summary**
  - One of the most powerful extensibility points, and likely the most used one, is the action filter. 
    - You can overwrite an existing action filter to add custom functionality, or you can create your own filter. 
    - The action filter enables you to get into the processing stack before the action gets executed, or immediately after the action gets executed.
  - You can add a result filter, which is like an action filter but for action results. It has two methods: OnResultingExecuting and OnResultExecuted.
  - You can create a custom controller factory that enables you to make nontraditional decisions about how your controllers are constructed. This kind of approach is useful when you need to pass inc ertain information such as dependencies or runtime references. 
  - Overriding a view engine enables you to interject additional business logic into the HTML rendering. If your needs are more extensive than adding behaviour, such as replacing behaviour or wanting to support a syntax different from Razor or ASPX, you can create and register your own custom view engine.
  - Model binding is a vehicle for facilitating one- and two-way communication between the view/form items and a model in the application. Sometimes there is no direct correlation between the two values. In those cases, custom model binding is a good way to pull that out of a controller and make it testable and reusable. 
  - The default route handler gives the developer a lot of flexibility in defining routes, but sometimes you might need additional or different functionality. ASP.NET MVC 4 enables you to create custom route handlers that support your need to interpret URLs differently. As with the other customization choices, you can either override the existing default functionality to add your required logic or you can comepletely replace it. 

## Reduce network bandwidth
- One method is to ensure that only needed items are sent to the client. 
  - You should clean up old, unused JS files or methods that are still inked, and remove unused or redundant styles in your CSS files.
  - Also can take advantage of bundling and minification, which are JS and ASP.NET MVC features that remove extraneous information from scripts and merge them into a single script for download.
  - Consider compressing the data you are transferring to the browser if it's still too large after doing the above.
- After minimizing size of the content downloaded to clients, you can look at minimizing the effect of the network.
  - Minimize number of network hops that occur between client and server.
  - A content delivery network (CDN) can help; it removes some of the network hops between client and server and takes a portion of the load off your server.
- **Bundling**
  - Create single file form multiple files to limit number of connections needed for downloading files. 
  - Will save download time only the first time the file is downloaded (since usually cached afterwards)
  - However, you have slightly increased the amount of time it takes to find the necessary function or other item from within that file. 
  - This increase takes place every time the file is accessed, not just the first time it is downloaded. You get a one-time gain in network speed for some contiual impact on acess performance. It becomes a balancing act.
  - BundleConfig.cs
  - The bundle functionality gives you another path to put this script into your page: @BundleTable.Bundles.ResolveBundleUrl()
    - This code creates the script link for you and generates the hashtag for the script. 
    - This means the browser will store the script longer and the client will have to download it fewer times. 
    - With the hashtag, browsers get the new script only if the hashtag is different or if it hits the internal expiration date, which is generally a year. 
- **Minification**
  - Runs through JS files and CSS files and removes all instances of extraneous content such as comments and whitespace, and replaces variable names. 
  - Purpose is to make the file smaller so it is faster to download.
  - Unlike bundling, there is no sideeffects or extra costs for using minificaiton.
  - Enabling minification is simple; youc an enable it in your configuration file by setting the compilation elements debug attribute to false.
    - <compilation debug="false" />
- **Compressing and decompressing data**
  - Accept-Encoding: gzip, defalte (These are in the headers of the request)
  - These are compression types. The easiest way to take advantage of this is to confiugure compression in IIS. 
  - The server automatically compresses files before sending the response to the Internet. 
  - Can compress static or dynamic content, can also set a minimum size of the file before the server will compress it etc.
  - You can have your application zip up content before providing it to users, which is appropriate for needs such as reports and documents. 
    - Users have to decompress the compressed file.
  - IIS enables you to configure your web server to send compressed content to users whose browsers accept gzipped content.
  - There is a tradeoff between sending a smaller file and the extra amount of time it takes on the client to unzip the content.
    - It becomes a choice between savings in download time vs the extra client cost to decompress the file. 
- **Planning a content delivery network (CDN) strategy**
  - CDNs provide a way to distribute your content from sources other than your server. The delivery nodes might be within your network or external, but they are not part of the server system running your .NET MVC app.
  - Server several purposes: 
    - Take the work load of serving images, CSS files, JS files, and other static content off your app server. 
    - Get content closer to the client. The client can download the files from much closer locations (nodes closer).

## TODO: Look at the Troubleshoot and debug web applications chapter.
- **Summary**
  - The Performance Wizard in Visual Studio enables you to configure profiling to capture information on CPU usage, memory usage, and resource/threading information.
  - The Visual Studio profiler performs a complete trace of all the calls in an application enabling you to monitor and evaluate the process and logic flow within your app. Can find problems such as methods being called too often and other potential performance impacts.
  - Performance Monitor comes with Windows Operating System and provides information about many different characteristics of the running application.
  - Tracing is functionality in the System.Diagnostics namespace that enables you to write information to one or more TraceListeners. 
    - A listener writes info to a text file, XML file, or another format. 
    - Can call the functionality to write this info by using Trace object and the static methods for Write, WriteIf, WriteLine and WriteLineIf or Create a custom TraceListener if necessary. 
  - Logging third party tools: NLog and log4net. 
  - Code contracts are a way to make a method responsible for defining and publicizing its own internal conditions (pre, invariant, and post).
    - Throw exceptions if their rules are violated. 
  - Health monitoring is a system that is part of ASP.NET that tracks various events ocurring within your application. 
    - You add it through configuration. 
    - Also can capture limited information about an applications state as it runs. 
    - There are specific mappings for all errors, infrastructure errors, processing errors, failures and other events. 

## TODO: Look at Design an exception handling strategy
- **Handling exceptions across multiple layers**
  - Multiple layers require you to understand layer rules.
  - A layer should know only about the layer it communicates with, and it should have no knowledge about layers that might be calling it. 
  - A traditional three-tier app has a data layer, business layer and a user interface layer. The data layer doesn't know anything about the other layers, the business layer knows about the data layer but nothing about the UI layer, and UI only knows about the business layer. 
  - The same approach is appropriate
- **Displaying custom error pages, creating your own HTTPHandler, and setting Web.config attributes**
  - Need to determine which errors will have custom pages and what kind of information should be displayed on the pages: one to handle 404 page errors and a more generic error display page. 
  -  The Global.asax page is one of the ways you can support custom error pages. Can use the Application_Start method and the Application_Error methods, a global error handler that is called when an unhandled error makes it through the application stack. 
- **Handling first chance exceptions**
  - First chance exceptions are exceptions before they have been handled by an error handler. 
  - Every error that occurs in an application begins the error-handling process as a first chance exception. 
- **Summary**
  - What you need to do with the exceptions varies based on where in your application the error occurs. Typically, a layer in a multilayer application handles two sets of exceptions: its own and the exceptions from the layer below it in the stack. A layer does not handle exceptions thrown in the data layer. Those exceptions should be handled by the business layer.
  - You can create custom error pages for display in your application. You can create custom error pages like any other controller/view combination. You define the error handling controller and then you create the view(s) to manage the various errors. You can add the pointers to the error files in both code and in configuration.
  - First chance exceptions are exceptions that are immediately thrown, before they have been handled. You can add first chance exception handler to your application.
    - Can add logging or other diagnostic or cleanup items in this handler.
    - However, you need to make sure the first chance exception handler is exception-free, as exceptions will cascade and could easily cause a stack overflow. 

## **TODO: Look at Testing chapter again**
- **Creating and running web tests**
  - ASP.NET allows you to do both Load and performance web testing:
    - Add a web test and load project to your solution
    - Configure the test flows taht will be used to perform the test
    - Easiest way to provide the test flow is to record a series of actions taken within your web application.
    - Should have a .webtest file available in your new project. Open the file and start recording and exercise the application as needed. The system will record your path. 
    - You can use the recorded test as needed, based on the type of load test you will run. The load test runs a set of web tests. 
- **Types of load tests**
  - Constant
    - Enable you to set a constant number of users.
    - The testing process uses the same users through the entire test run.
    - Using a large number of users in constant mode, however, can place an unrealistic demand on the application and servers.
  - Step
    - steadily adds users to the testing process. 
    - There are four values you need to consider when using a stepped performance approach:
      - Initial user count
      - Maximum user count
      - Step duration
      - Step user count
  - Goal-based
    - Similar to step-load test, however, differs because it doesn't count the running users as the key point of information. Rather it uses the user count as a way of getting to other goals: percent of CPU usage and percent of memory usage etc. i.e. With goal-based load tests you can run various types of tests, such as determining how many concurrent users will push the CPU to 75 percent usage. 
    - The data acquired from ogal-based load tests is important over time. 
- **Test planning**
  - Smoke
    - Generally puts a light load on the application over a shorter period of time.
    - You might use a smoke test immediately after deploying to a new environment to make sure the application runs correctly.
  - Stress
    - Runs your app under a heavy load for a long time to reveal your apps behaviour under stress. Where a smoke test may last a matter of minutes and is generally more concerned with breadth of coverage than depth of coverage, a stress test generally lasts hours and is concerned with both breadth and depth of test coverage.
  - Performance
    - Tests the responsiveness of your application. This kind of test keeps careful records of when requests started, when the first piece of data is returned to the client, and the length of time and amount of data that was transferred.
  - Capacity planning
    - Uses a testing process to support the correctness of the application and to help plan for deployment. This approach uses the number of expected visitors as a metric and applies it to the application
    - This will be combined with the CPU limits, to determine how many or what type of servers need to be used to support the expected usage. 
- **Summary**
  - Unit tests enable you to validate your app repeatedly.
  - Unit tests focus on a single method and attempt to test only taht function without any dependencies. 
  - Integration test examines more than one item at a time, such as retrieving known information from a database and performing some business logic on it. 
  - Can use VS Ultimate 2012 edition to create and run web performance and load tests. 

## TODO: Look at Debug a Windows Azure application chapter
- **Collecting diagnostic information**
  - Tracing, debugging errors, watching on performance issues, and monitoring system resource usage. 
- **Choosing log types**
  - The diagnostics library within Windows Azure offers a tremendous number of tools designed to support trace and debug process flows within applications running in Windows Azure. 
- **Summary**
  - When you have deployed your app into Windows Azure, you will find your traditional ways of gathering diagnostic information are not available or give unexpected results. To compensate this, Microsoft provided a special Windows Azure-specific diagnostics API, Microsoft.WindowsAzure.Diagnostics.
  - Getting diagnostics running in your Azure-deployed app requires several steps. The first is adding information into your ServiceDefinition.csdef file so that you can import the Diagnostics module. You also need to make sure that information is added into your ServiceConfiguration.cscfg file so that the diagnositc module can access databases or other business needs.
  - After diagnostics are fully available in your Windows Azure application, you can either code your calls to the diagnostics or use the built-in event monitors. To cinfugre the event monitors, create a new file: Diagnostics.wadcfg. This file contains the configuration entries that will set up the appropriate counters. After diagnostic information is being saved, you can programmatically download the information from the server or get it on demand.
  - Azure also enables you to configure the role to run IntelliTrace on the app. You need to deploy the app using VS Ultimate 2012, and amke configuration chagnes during publish process. After IntelliTrace is logging the role, you can download and review this info through VS. 

## TODO: Look at Chapter Summary.

## TODO: Design and Security Chapter

## TODO: Authentication
- Authenticating users
  - There are 2 parts of Authentication: IIS and the .NET framework. They work together to manage the authentication process.
  - **IIS authentication**
    - Challenge-based authentication method
      - Occurs when client must respond to server's demand for credentials. 
      - Examples of these include Basic, Digest, Windows, Client Certificate Mapping, and IIS client certificate mapping. 
    - Login redirection-based authentication method
      - When the client sends login credentials to the application without being required by the server. 
      - The application takes the login information and uses it to determine where the user should be redirected. Forms authentication is the primary example. 
    - Anonymous authentication
      - Does not require any crednetials from the user. 
  - **.NET authentication**
- **Summary**
  - IIS is the primary mechanism for authentication because it comes bundled with seven providers. However, with IIS 7, only Anonymous authentication provider comes installed by default. If you want to use any of the other providers, you need to install them separately. 
  - **Anonymous authentication**
    - users do not require login credentials
  - **Basic authentication**
    - requires credentials that are validated against the domain, but the information is not sent securely
  - **Digest authentication**
    - Similar to Basic, but the credentials are sent hashed. 
  - **Forms authentication**
    - One of the most commonly used, because it enables you to authenticate users whatever way you want. 
  - **Windows authentication**
    - Uses credentials from Windows logged-in users and sends them with the HTTP request.
  - **Client Certificate authentication**
    - Matches certificates between the client and the server and uses it to access user information.
  - **IIS Client Certificate authentication**
    - Allows validation against both Active Directory and the local server store.
  - The main way to enforce authentication in ASP.NET MVC 4 is through the use of attributes. The Authorize attribute tells the system that any users calling the controller or the action need to be authenticated. The AllowAnonymous attribute tells the system that it is permissible for the users to not be authenticated. 
  - Can use custom authentication in MVC 4. The best method is to implement the IIdentity and IPrincipal interfaces (Enables you to work with all the default authentication mechanisms).
  - MVC 4 introduced SimpleMembership and SimpleRoles. These enable you to customize access to data storage by specifying the table, unique identifier, and username in the initialization. 
  - You can create custom membership providers by subclassing AuthorizeAttribute or by deriving from the Forms authentication provider and overriding the applicable methods.
  - Choosing the appropriate authentication type depends on several factors. The primary factor is the user store that contains the login info that will be used to verify the website user, If you are using an Active Directory-based authentication system, you should use one of the standard challenge-based methods. 
    - If you are using a different technology for your user store, you need to use an overridden provider or a custom provider. 
    - If you do not have a provider or need a special one just for the website, Forms authentication can be the best way to implement your authetnication requirements. 


## TODO: Configure and apply authorization 
- **Summary**
  - To add roles, you must create a UI and invoke the Roles API. When launching a new app, you can ensure role creation occurs as part of the database creation script. If you will support user-created roles in your app, you need to write this functionality yourself. 
  - You can check the validity of roles in several ways:
    - Through attributes on the controller or action [Authorize(Roles="Admin")], or in code by using IsUserInRole or GetRolesForUser: RoleProvider.GetRolesForUser, HttpContext.User.IsInRole, and RoleProvider.IsUserInRole. 
    - Can create custom role providers by implementing RoleProvider. Custom role providers enable you to manage role access when standard role providers don't meet your needs. You might want to create a custom role provider to get information from nonstandard databases, or when you want to use a different database schema from the standard .NET implementation. 
    - When your app consumes WCF services, you must often manage authentication between your app and the service. To manage authetnication, you can use the Credentials collection on the client proxy (created by using the Add Service Reference command in Visual Studio). You can create a credential using a user name and password, or the WindowsIdentity fromt he principal. 


## TODO: Design and implement claims-based authentication across federated identity stores 
- **Summary**
  - Windows Azure Access Control Service (ACS) enables you to implement federated authetnication. The four primary participants int he ACS authentication process are the relying party (your app), the client browser, the identity provider, and the ACS.
  - OAuthWebSecurity.VerifyAuthentication is the main process used to create the external callback for authentication. As you are calling it, you can determine whether you want to create a persistent cookie. This cookie will let you determine in subsequent calls whether the user is still authenticated. 
  - WIF is part of the .NET framework and can be used to build identity-aware applications. You can use it to manage any of the built-in token handlers, as well as the tokens that provide the information.
  - You can create custom tokens as well as custom token handlers to read tokens. Custom token handlers are useful wheny ou need to create custom tokens. They are also necessary when you use a token where support is not already built in to the framework, such as SWT and JWT. 

## TODO: Manage data integrity
- **Summary**
  - Encryption is process of turning text input into an illegible format that is decipherable only to applications that have the decryption key. 
  - Salting is the process of adding a random string to input text before the hashing or encryption process. A salt adds unpredictability to the conversion from text to hash to help prevent unauthorized access of the text. 
  - Symmetric and assymmetric algorithms are used for encryption. 
    - Symmetric encryption uses same key to encryp and decrypt data. 
    - Asymmetric encryption uses two different keys: A public key is widely distributed and is used for encryption, whereas a private key is kept on the decryption side and is used with the public key to decrypt the data.
  - When using encryption, protect the keys by storing them separate from the encrypted data. You should switch your keys on a defined basis, which includes redefining the process of decrypting and encrypting data. 
  - You can encrypt sections fo a web.config file using the aspnet_regiis.exe command with the -pe, -app, and -prev options. Encrypting areas of a web.config file protects the file's content in case the file is served to users inadvertently. Decryption of the file can be handled with the -pd option.
  - Signing app data provides authentication, authorization, and nonrepudiation. This enables you to verify your communications partner and gives you confirmation that the signed application data came from your partner rather than someone else. 

## TODO: Implement a secure site with ASP.NET
- **Summary**
  - SSL is used by browser and server to establish a secure communications. Uses a PKI in which the public key is bound through a trusted 3rd party or CA. 
  - Before you use SSL, ensure your web server has HTTPS: bindings enabled. You then need to send identifying information and your server-created public key, with the Certificate Signing Request (CSR) to the certificate authority for validation. After your information has been validated and your request approved, the CA will send you a data document containing your certificate that you can load into your server for usage. 
  - Storing your user passwords should be salted and hased before persistence. 
  - The AntiXSS lib takes an accepted-list approach to encoding characters for display to prevent XSS attacks, in which hackers injects their own JS into a website. This prevents embedded JS.
  - SQL injection attack occurs when a hacker inserts SQL commands into unprotected queries in an attempt to alter or view data or cause other damage. You can control this by parameterizing your queries using SQLParamaters. Entity SQL has the same risk. Linq-to Entities does not have the same problem because it uses the object  model. 
  - CSRFs play on the trust that a server has for its clients. It happens when a user takes information from the server, alters it, and then sends it back to the server. This could enable the user to affect orders placed by another user, or add things to a shopping cart without paying for them. The AntiForgery method ont he form and the ValidateAntiForgeryToken on the controller/action work together to make sure that the page returned to the server is the same as the one that was sent to the client. 
