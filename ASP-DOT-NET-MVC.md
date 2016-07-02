# ASP.NET MVC

## Model
- Handles Business logic.
- Manages data access and performs the business logic on the data.
- **Domain Model approach:**
  - When the object you are using describes the data you work with in the middle tier of that application. i.e. using passing Entity framework objects to a view for display. 
- **View Model approach:**
  - Describes the data being worked on in the presentation layer.
  - Any data you present in the view is found within the properties of the view model class, which represents all the data the controller transmits to the view after processing the request. 
  - A view model is generally the result of aggregating multiple classes into a single object.
- **The Input Model approach:**
  - represents the data being uploaded to the server from the client with each individual HTTP request. 
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