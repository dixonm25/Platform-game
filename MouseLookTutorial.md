# Mouse Look Tutorial
This tutorial shows how to look around with your mouse. Code was taken from - https://youtu.be/_QajrabyTJc?si=ENicKA1AU4kst5sj

# 1. Create a new scene
After creating a new scene, create an empty objeect called ``` Player ```.

Add a CharacterContoller component to the empty object.

Add a cylinder and make it a child of the empty object to give the player graphics - remove the collider on the cylinder.

Reset the transform on the cylinder so that it appears inside the empty object. Then resize so that both the cylinder and empty object are the same size.

# 2. Add a camera to the ``` Player ``` object
Drag the main camera onto the ``` Player ``` object.

Reset the cameras transform so that it appears inside the Player.

Move the camera up inside the Player object so that it is at eye level.

<img width="192" alt="image" src="https://github.com/dixonm25/GpProject1/assets/146852548/c52d5646-184a-4eb0-8e19-dd7953a6ad8d">


# 3. Add camera mouse movement
Add a script component to the camera attached to the Player called ``` MouseLook ```.

To get camera movement we need to detect inputs from the player.

To do this we have to add the following code into the ``` Update ``` function.

```ruby
float mouseX = Input.GetAxis("Mouse X") * mouseSensitivity * Time.deltaTime;
float mouseY = Input.GetAxis("Mouse Y") * mouseSensitivity * Time.deltaTime;
```
and this code at the start of the script 

```ruby
public float mouseSensitivity = 300f;
```
This code allows us to move the camera with our mouse on the X and Y axis, as ``` "Mouse X" ``` and ``` "Mouse Y" ``` are already preprogrammed into Unity to allow us to move on the given axis, and multiplying it by ``` mouseSensitivity * Time.deltaTime ``` allows us to move it on a fixed speed.

# 4. ``` Player ``` rotation with camera
Now that we can look around, we need to rotate the ``` Player ``` object to match the camera movement inputted by the player.

```ruby
public Transform playerBody;
```
We add this at the start of the script so that it can be called upon later. This also lets us like the player model to this script.

```ruby
playerBody.Rotate(Vector3.up * mouseX);
```
This lets us rotate on the X axis.

```ruby
float xRotation = 0f;
```
We could do the same thing that we did to rotate on the X axis, but as we will need to clamp our rotations on the Y axis we need to define ``` the xRotation ```.
```ruby
xRotation -= mouseY;
```
The reason we do ``` -=  ``` is so that the rotation doesn't go the opposite way.
```ruby
transform.localRotation = Quaternion.Euler(xRotation, 0f,0f);
```
After this we should be able to rotate on the Y axis, but to clamp we need to add the following piece of code:
```ruby
xRotation = Mathf.Clamp(xRotation, -90f, 90f);
```
This stops the rotations on the Y axis from going beyond the distance of -90 and 90 degrees.

Here is what the gameplay should look like:

https://github.com/dixonm25/Platform-game/assets/146852548/a6c4813e-8987-41c0-96fc-cb0707d74d5b

