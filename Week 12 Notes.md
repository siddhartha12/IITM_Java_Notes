# L12.1 - Graphical Interfaces and Event-driven programming
* How do we design graphical user interfaces
* Multiple applications simultaneously displayed on screen
* keystrokes, mouse clicks have to be sent to appropriate window
* in parallel to main activity, record and respond to these events
	* web browser render current page
	* clicking on a link loads a different page

keeping track of events:
* remembere coordinates and extent of each window
* track coordinates of mouse
* os reports mouse click at (x,y)
	* check which windows are positioned at (X, y)
	* check if one of them is "active"
	* Inform that window about mouse click
* Tedious and error prone
* programming language support for higher level events
	* runtime support for language maps low level events to a high level events
	* OS reports low level events: mouse clicked at (x, y) key "a" pressed
	* Program sees high level events: button was clicked, box was ticked

support for events
* Programmer directly defines components such as windows, buttons that "generate" high level events
* each event is associated with a listener that knows what to do
	* eg: clicking close window exits application
* programming language has mechanics for 
	* describind what types of events a component can generate
	* setting up an association between components and listeners
* Different events invoke different functions
	* Window frame has Maximize, Iconize, Close
* Language "sorts" out events and automatically calls the correct functions in the listener
* Example: A button with one event, press button
* Pressing the button invokes the function buttonpush() in a listener
* We have set up an association betwen button b and a listener buttonlistener m
```
Button b = new Button();
MyClass m = new MyClass();
b.add_listener(m); //Tell b to notify m when pushed
```
* Communicating each button push to the listener is done automatically by the runtime system
* Information about the button push event is passed as an object to the listener
* buttonpush() has argument
	* Listener can decipher source of event, for instance

Recall timer
* Myclass m creates a Timer t that runs in parallel
* Timer t notifies a Timerowner when it is done, via a function timerdone()
* Abstractly, timer duration elapsing is an event, and Timerowner is notified when the event occurs
* Abstractly, timer duration elapsing is an event, timerowner is notified when event occurs
	* in button, notificaton is handled internally automatically
* In principle Timer t could be passed a reference to any object that implements Timerowner interface

# L12.2 - SwingToolKit
Event driven programming in java
* Swing toolkit to define high level components
* built on top of lower level event handling system calling AWT
* relationship between components generating events and listeners is flexible
	* one listener can listen to multiple objects
		* three buttons on window frame all report to common listener
	* One component can inform multiple listeners
		* Exit browser reported to all windows currently open
* Must explicitly setup association between component and listener
* Events are lost if nobody is testing

Example: A button that paints its background red
* JButton is swing class for buttons
* Corresponding listener class is ActionListener
* Only one type of event, button push
	* Invoked, actionPerformed() in listener
* Button push is an ActionEvent
```
public class MyButtons{
	private JButton b;
	public MyButtons(ActionListener a){
		b = new JButton("MyButton");
		// set the label on the button
		b.addActionListener(a);
		// associate an listener
	}
}

public class MyListener implements ActionListener {
	public void actionPerformed(ActionEvent e){ ... }
}

public class XYZ {
	MyListener l = new MyListener;
	Mybuttons m = new MyButtons(l)
}
```
* So far we have just made a button, not displayed it yet

Embedding the button in a panel
* to display we use something called a frame, a portion of the frame is known as a panel, so its like panel is the container for button and frame is the container for panel
* Embed the button in a panel - JPanel
	* first import required Java Packages
	* the panel will also serve as the event listener
	* create the button, make the panel a listener and add the button to the panel
* Embed the panel in a frame
* Corresponding listener class is WindowListener
* JFrame generates seven different types of events
	* Each of the seven events automatically calls a different function in WindowListener
* Need to implement windowClosing event to terminate the window
* Other six types of events can be ignored
* JFrame is complex, many layers
* Items to be displayed have to be added to ContentPane

The main function for the above
* Create a JFrame and make it visible
* EventQueue.invokeLater() puts the swing object in a separate event despatch thread
* ensures that GUI processing does not interfere with other computation
* GUI does not get blocked, avoid subtle synchronization bugs
```
public class ButtonTest{
	public static void main(String[] args) {
		EventQueue.invokeLater(
			() -> {
				JFrame frame = new ButtonFrame();
				frame.setVisible(true);
			}
		)
	}
}
```

# L12.3 - More swing examples
Connecting multiple events to a listener
* one listener can listen to multiple objects
* A panel with three buttons, to pain the panel red, yellow or blue
* Make the panel listen to all three buttons
* Determine what colour to use by identifying source of the event
	* Keep the existing colour if the source is not one of these three buttons

Multiple listeners for an event
* Two panels, each with three buttons: Red, Blue, Yellow
* Clicking a button in either panel changes background colour in both panels
* Both panels must listen to all six buttons
	* However each panel has references only for its local buttons
	* How do we determine the source of an event from a remote button
* Associate an ActionCommand with a button
	* assign the same action command to both Red buttons
* Choose colour according to ActionCommand
* Need to add both panels as listeners for each button
	* Add a public function to add a new listener to all buttons in a panel
* Add both panels to the same frame

Other elements
* JCheckbox - box that can be ticket
* one action - click the box
	* Listener is ActionListener
* 