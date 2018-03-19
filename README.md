## What is dark viper?
Following the principle of Clean Architect, it’s an iteration on the VIPER architecture. It adds the necessary services that a mobile app needs.

## Dark explained
DARK stands for Data , API, Request and Konfigurator.
Yes I know configurator is spelled with a C, but come on DARK VIPER has a cool ring to it!

## How to implement
### Konfigurator
Every VIPER module needs an initializer, that’s were the Konfigurator enters. It initializes all the classes and adds the connections for the communication to work.
Normally the in first module, let’s say an Authentication module, the Konfigurator would be called in the AppDelegate. 

The main communication between what VIPER and what DARK has to offer is between the Interactor and the Request handler.
Request Handler
With the Request Handler, all the logic for http requests, persistent data, and turning the JSON responses into Entities, is removed from the Interactor. The Interactor job is to send a request to get data to the Request Handler. Where the data comes from, http request or persistent DB, is of no concern of the Interactor. 
It’s the Request Handler's job to see if there is any data in the local DB or if an http is needed in order to get data.  Normally the Request Handler will request the API component for new data, with whatever parameters are needed, and at the same it will request the Data Store for any persistent data that can be showed immediately to the user. Then when the API component returned with new data, the Request Handler will send it to the Data Store to add or update the local DB and send the new data to the Interactor.

### API Component
The API component deals with all network requests. It should remain agnostic about the result, returning it to the Request Handler for treatment. If the request fails, the API component should have a worker that treats all network errors and return them to the Request handler, that in turn sends the error to the Interactor.
It would be in the API component that the developer could use a third-party framework for networking. 

### Data store
The Data Store focuses on creating Entities, and creating or saving those entities. Any conflict resolution logic is dealt with in the Data Store.
The Data Store will save information in the KeyChain/UserDefaults/File, or in a DB like Core Data / Realm.

### Entities
Entities are representation models (Class/Struct). Do not mistake these Entities with persistent objects (Core Data- NSManangedObject/ Realm - Object). The Data Store will retrieve data from the DB, and turn those results into Entities. This way, there is no need to bloated Entities that are not need.
A practical example is when parsing JSON objects to a NSManagedObject, you will parse all the paramenters, and then store them locally. When retrieving data, the Requestor may only need a portion or a join of various  NSManagedObject so it turns them into an Entity. 

### Interactor
The Interactor's purpose is to handle all the business logic. It receives information or actions from the Presenter. Then sends off requests to the Request Handler, filter data, or lets the Presenter know he has to route to a new ViewController. The communication is made from the Presenter and then to the Presenter again.
Any errors that the Data Store or API component return, its the Interactors job to know what to do with those errors.

### Presenter
The Presenter's purpose is to receive inputs from the ViewController and then, send those inputs to the Interactor for data or send a action to the Router to change the ViewController. Also receives data from the Interactor, and uses that data to populate the ViewController. 

A practical example is populating a UITableViewCell. The ViewController has a UITableView and is it´s Data Source. The Interactor sends data to the Presenter to present to the user, the Presenter in turn sends an action to the ViewController to "reload tableview".  But when a cell is dequeued in the "cellForRowAtIndexPath", The ViewController calls the Presenter to populate that cell.

### ViewController
The ViewControllers is UI aspect of the app. While the Presenter says what to show, the ViewController says how to show it. 
All user inputs come from the ViewController. But the ViewController is agnostic about what to do with those inputs. The ViewController sends those inputs to the Presenter, that in turn will know what to do.
A ViewController manages various child views, from UIViews to UITable and UICollection views. 




# dark_viper_generamba_template
This is a tempate to generate files for DARK VIPER architecture in Generamba.
