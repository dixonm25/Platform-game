# Door and Key Tutorial
This tutorial shows how to make a simple door and key system. Code was taken from - https://youtu.be/hWbdaihafjo?si=NrCTft2O_1yOdv2H

# 1. Add a door and key object
Add and scale an object that will represent your door and call it ```Door```.

For the door to open on a fixed point we have to add a hinge.

We will add a empty game object and call it ```Hinge```. Move this game object to the far side of the door object where you want the door to pivot open from.

<img width="282" alt="image" src="https://github.com/dixonm25/GpProject1/assets/146852548/7a7fe8b9-05a3-4b4b-8ea7-7c531ebb07c2">

In the hierachy tab drag the door onto the hinge to make it a child object of the hinge.

To make the door rotate on the hinge, on the hinge object change the "center" to "pivot" under the scene tab.

<img width="243" alt="image" src="https://github.com/dixonm25/GpProject1/assets/146852548/64195c77-7041-4e92-84f5-c8932ac86918">

Now add and scale an object that will represent your key and call it ```Key```.

# 2. Make animation for door
On the hinge object add the animation component and bring out the animation timeline.

Click the create button on the timeline and name the animation ```OpenDoor```.

Start recording the animation and then rotate at the hinge to open door how you want it animated.

After recording, add the animation to the tab under the Inspector tab so that it call be called on later.

# 3. Add a script to the hinge
Add a script called ```OpenDoor``` to the hinge.

To get the animation to play when we interact with the object we have to add the following code that will call the game object with the animation:
```ruby
public Animation hingehere;
```
To get the door to open after we interact with it we have to call for the animation after we press a certain key:
```ruby
void OnTriggerStay()
    {
        if (Input.GetKey (KeyCode.E)) 
            hingehere.Play ();
    }
```
Now back in the Unity Editor call the hinge with the animation under the "hingehere" tab.

Add a box collider component to the hinge object and scale it so that the player has no difficulty try to interact with the door so that the animation can run.

<img width="203" alt="image" src="https://github.com/dixonm25/GpProject1/assets/146852548/c1a5b8bc-7e0b-40e9-aeb3-175e5e59a4a5">

Before testing tick the "Is Trigger" box as we are triggering an event. And uncheck the "Play Automatically" box as we don't want the animation to instantly play.

# 4. Add a script to the key 
Now we will add a script to the key so that the player has to collect the key before they can open the door, this script will be called ```PickKey```.

To do this we have to enable the box collider on the door after we pick up the key. So first we will uncheck the Box Collider on the door so that we can later enable it after picking up the key.

After that we will call for the hinge object, as it it connected to the box collider for the door, and enable the box collider on the door after we have picked up the key.
```ruby
public Component doorcolliderhere;

void OnTriggerStay()
{
    if(Input.GetKey(KeyCode.E))
    doorcolliderhere.GetComponent<BoxCollider> ().enabled = true; 
}
```
In the Unity Editor call the ```Hinge``` at the "doorcolliderhere" tab so that the box collider can be enabled.

Before testing increase the size of the box collider on the key to make it easier to interact with. Tick the "Is Trigger" box as we are triggering an event again.

<img width="278" alt="image" src="https://github.com/dixonm25/GpProject1/assets/146852548/c58e808b-e554-4c7e-bac7-a3065625f0ce">

Now to get the key to disappear after we interact with it we have to add some code to the ```PickKey``` script.
```ruby
public Component doorcolliderhere;
public GameObject keygone;

void OnTriggerStay()
{
   if(Input.GetKey(KeyCode.E))
   doorcolliderhere.GetComponent<BoxCollider> ().enabled = true;

   if (Input.GetKey(KeyCode.E))
   keygone.SetActive (false);
}
```
This added code will call on the game object we link to ```keygone``` and deactivate it after it has been interacted with.

So in the Unity Editor drag the Key object onto the ```keygone``` tab.

Right now when we interact with the door after we have collected the key, the box collider stays enabled so the door can constantly be opened. To fix this we will disable the box collider again after we interact with the door. To do this we add some code to the ```OpenDoor``` script.

```ruby
public Animation hingehere;
public Component doorcolliderhere;

void OnTriggerStay()
{
    if (Input.GetKey (KeyCode.E)) 
        hingehere.Play ();

    if (Input.GetKey (KeyCode.E))
    doorcolliderhere.GetComponent<BoxCollider>().enabled = false;
}
```
In the Unity editor call the ```Hinge``` at the "doorcolliderhere" again so that the box collider can be disabled again.

Here is what the gameplay should look like:

https://github.com/dixonm25/Platform-game/assets/146852548/b4781e33-e245-4f04-bc5c-08d89dfc7a37

