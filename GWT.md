# GWT

#### Widget
* Widget, every UI element is a widget
    * can be a single DOM element or complex (use many widgets inside)
* Panel is a widget that contain others widgets and panels
    * used to define layout of UI
    * contains other panels and widgets 
    * when possible we should use uibinder.xml to describe the structure
* Composite is a widget with hidden implementation, acts like a simple widget, composite pattern => contains single and complexe widgets inside

###### Panels:
* RootPanel is the top panel
    * RootPanel.get() => singleton, return the <body> of HTML
    * RootPanel.get(String id) => return the element of HTML with given id
* RootLayoutPanel
    * container for all others panels:
        * DockLayouPanel
        * SplitLayouPanel (identical to DockLayoutPanel but draggable)
        * StackLayoutPanel
        * TabLayoutPanel
* LayoutPanel
    * the most general layout mechanism
* AnimatedLayout
    * can animate their child
* layout panels should be used to describe the structure of HTML and
	concrete panels (like HTMLPanel) and widgets should be used in
	describing real elements
* FlowPanel
* HTMLPanel
* FormPanel
* ScrollPanel
* PopupPanel, DialogBox
* Grid and FlexTable 

###### Widgets
* Examples of widgets:
    * Button
    * TextBox
    * Tree
    * RichTextArea
* Popular ways to create custom widgets:
    * You can bundle together existing widgets and create a Composite widget
    * You can write GWT bindings to an existing JavaScript widget
    * You can create your own widget from scratch using either Java or JavaScript
* Custom Composite widget:
    * Implement Composite design pattern,
    * Extend the Composite class (or ResizeComposite class if it contains a layout panel)
    * Call the initWidget method, passing in as a parameter the widget that acts as the parent
    * Doesn’t inherit any functionality or interfaces, you have to indicate implemented interfaces
* Lifecycle of widget
    * Create
        * In Java we use “new Button()”
        * GWT creates corresponding DOM element <button type=”button”></button>
        * Element is not visible in application until we add it
    * Add (in DOM we call it Attach)
        * In Java we use add()
        * onAttach() is call inside, we shouldn’t override this method:
        * We can override onLoad(), empty by default
        * AttachEvent is fired, we can add AttachEventHandler to widget
    * Remove (in DOM we call it Detach)
        * In Java we use remove()
        * When we remove widget, all his children are also removed
        * Unplug the widget from event-handling system
        * onDetach() is called, we shouldn’t override this method
            * We can override onUnload()
            * AttachEvent is fired, we can add AttachEventHandler to widget
    * Destroy
    
###### Cell Widgets (data presentation widgets) 
* These widgets are designed to handle and display very large sets of data quickly, 
* design follows the flyweight pattern where data is accessed and cached only as needed, and passed to flyweight Cell objects
* FLYWEIGHT design pattern
* Populating with data
    * Direct approach is to call setRowData(...)
    * Use DataProvider, the table will keep eye for any change on the model
        * ListDataProvider - Any changes to a backing list held on the client side are reflected in the widget.
        * AsyncDataProvider - Data is retrieved usually from a server source.
        * CustomDataProvider - You manually handle any RangeChangeEvents from the widget

#### Resources
* ClientBundle
    * An interface that defines methods to access any resources that you choose to load it with
* DataResource
    * An interface, it’s primarily concerned with improving the cacheability of the data, without considering much else
* TextResource
* ImageResource
* CssResource

#### UiBinder
* @UiField(provided = true)
* @UiFactory
* @UiConstructor
* XML template and Java class to which it is bound
* GWT generates Java code base on XML and plug it into Java class, GWT compiler relies on convention that ui.xml file has the same name as Java class
* 3 steps are needed:
    * 1 Create an interface that extends UiBinder.
    * 2 Call GWT.create() to create a concrete class that implements the interface.
    * 3 Call createAndBindUI(this) to get the root element or widget.
* We can handle events using UiBinder, but only subclasses of GwtEvent
    * 1 Create a non private method that returns void.
    * 2 Add the @UiHandler annotation and specify the field name.
    * 3 Specify as a parameter the event type that you want to handle
    ```
    @UiHandler("btnLogin")
    void submitLoginForm (ClickEvent event) {
        if (validateEmail() && validatePassword()) {
            Window.alert("Logging in...");
        }
    }
    ```
* Styles:
    * <ui:style>
    * It's better to use css file instead
        
#### GWT RPC
* Server object has to implement Serializable
* DTO (data transfer object) which is on client side should implement isSerializable
* On client/shared side create service (and Async version of service)
    * Use annotation @RemoteServiceRelativePath("service")
    * Extend RemoteService
    ```
    @RemoteServiceRelativePath("service")
    public interface TwitterService extends RemoteService {
        public ArrayList<DataDto> getUserTimeline(String name) throws GTwitterException;
    }
    ```
* On server side create service implementation
    * Extend RemoteServiceServlet
    * Implement service interface defined on client side
* Deploy the servlet in web.xml file 
    * Define servlet and indicate url-mapping for it
    
#### GWT Event Handling
* __Native events__
    * Extends DomEvent class (which extends GwtEvent)
    * plain old JavaScript events, this includes clicks on a button, focusing on a checkbox or mouse movements over a widget…
    * you only have to add the event handler to the widget
    * events are fired automatically by GWT (when user for example click on the browser button), to prevent browser from default action:
    ```
    public void onBrowserEvent (Event evt) {
        evt.preventDefault();
        super.onBrowserEvent(evt);
    }
    ```
    * you can add handler on the fly by adding anonymous class or create a class that implement certain interface and create an instance of it
* __Logical events__
    *events specific to particular widget
    * Extends GwtEvent directly
    * GWT defined logical events: com.google.gwt.event.logical.shared
        * Attach event
        * Selection events: before selection and selection
        * Highlight event
        * Initialize event
        * Open event
        * Resize event
        * Show range event
        * Value change event
* __Event handler__
    * both native and logical events are handled by an event handler added to the object that could receive the event
    * to remove handler you have to add handler to HandlerRegistration
        * HandlerRegistration click = button.addClickHandler(...);
        * click.removeHandler();
* __Event propagation__
    * The difference in propagation of events in various browsers is whether events are bubbled or captured through elements:
    * __Event Bubbling__
        * Bubbling events will then trigger any additional event listeners found by following the Event Target's parent chain upward, checking for any event listeners registered on each successive EventTarget. This upward propagation will continue up to and including the Document.
    * __Event Capturing__
        * Event capture is the process by which an EventListener registered on an ancestor of the event’s target can intercept events of a given type before they’re received by the event’s target. Capture operates from the top of the tree, generally the Document, downward, making it the symmetrical opposite of bubbling.
    * GWT event model follows the Event Bubbling approach
    * Preventing event propagation, stopPropagation method:
    ```
    public void onClick(ClickEvent event){
        event.stopPropagation();
    }
    ```
* __Event handling__
    * _Internal_
    * Use sinkEvents(EventType…) 
    ```
    public class MyButton extends Label {
        MyButton(){
            super();
            sinkEvents(Event.ONCLICK);
        }
        public void onBrowserEvent(Event evt){
            switch(evt.getTypeInt()){
            case Event.ONCLICK:
                Window.alert("Hello!");
                break;
            }	
            super.onBrowserEvent(evt);
        }
    }
    ```
    * _External_
        * Extends proper interfaces and overrides required methods
    * Previewing and cancelling events
        * Previewing events is useful if you want to do something before the event gets handled - maybe you want to cancel it.
    * Preventing default actions
        * Use method event.preventDefault()

#### Creating your own events
* You have to define:
    * The event class 
    	* Extends GwtEvent class
    * The various interfaces related to handling the event: the Handler and Has-Handler interfaces
* Components can react to the same event without carrying references to each other. The main activities are first that the publisher fires an event. The event could contain data; any kind of data could be passed from publisher to subscriber. The subscriber in turn defines a handler that’s associated with the particular event. When a publisher fires an event on the EventBus, all of the registered subscribers to the same event will be invoked. The associated handler code will then be executed. Note that a publisher can also be a subscriber to events from itself or other components, and a subscriber can publish events that it’s responsible for.
* The EventBus implementation is related to the Observer design pattern, in which the subject (EventBus) keeps a list of its observers (subscribing classes) and notifies them when state changes occur by invoking one of their methods (the registered handler).
* The best approach is to use one event bus one per application and only for general event handling.
* Types of event bus:
    * All busses extend the abstract type EventBus
        * SimpleEventBus,
        * ResettableEventBus,
        * CountingEventBus,
        * RecordingEventBus.
    * General mechanism:
        * All subscribers and publishers have access to event bus, there are all observers
        * We create event bus at entry point of our application, as well as other components (widgets)
        * Subscribers add handlers to event bus:
        ```
        eventbus.addHandler(ChangeColorEvent.getType(),
            new ChangeColorEventHandler() {
                @Override
                public void onChangeColorSent(ChangeColorEvent event) {
                    String col = event.getColor();
                    changeColor(col);
                }
            }
	);   
        ```
        * Publishers fire the event as react to some user action
            * this.eventbus.fireEvent(new ChangeColorEvent(color)); 
