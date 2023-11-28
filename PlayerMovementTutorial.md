# Player Movement Tutorial
This shows how to add basic player movement and gravity to the player so we can jump. Code was taken from - https://youtu.be/_QajrabyTJc?si=ENicKA1AU4kst5sj

# 1. Add a floor
Firstly add a plane so that the player can move and doesn't fall later on.

Before we start adding code, add the "Player" tag to your player object and add the "Ground" Layer to any surfaces the player can stand on. These can be found at the top of the inspector for the object. 

<img width="349" alt="image" src="https://github.com/dixonm25/GpProject1/assets/146852548/4fbf07ee-37a8-4384-bf01-2cd922878dac">


# 2. Get player input
Add a new script component to the empty object of the Player and call it ``` PlayerMovement ```.

To get the input for the movement we will be using the built in input system for Unity by getting the "Horizontal" and "Vertical".

```ruby
float x = Input.GetAxis("Horizontal");
float z = Input.GetAxis("Vertical");

Vector3 move = transform.right * x + transform.forward * z;
```
The first part of code gets the input from the player, and the second part takes the input and applies it to the movement, also taking the direction the player is facing so that the player moves in the right direction and taking the local coordinates of the player.

# 3. Movement
For the Player to actually move we need to reference the "CharacterController" as it serves as the motor that controls the Player.

```ruby
public CharacterController controller;
```
```ruby
controller.Move(move);
```
As we want to be able to control the speed of the players movement we need to add a speed variable 

```ruby
public float speed = 12f;
```
After adding the speed variable we need to update the previous controller.Move code.
```ruby
controller.Move(move * speed * Time.deltaTime);
```
Now that the movement has been added, in Unity make sure to reference the CharacterController under the script component.

In the CharacterController you can change the 'Step offset' and 'Slope limit' so you can climb higher angled stairs and slopes.

# 4. Apply gravity and velocity

```ruby
Vector3 velocity;
```
This will store the velocity made by the player.
After that we need to add gravity onto the velocity on the Y axis.
So we will put this at the top of the code so that it can be called upon.
```ruby
public float gravity = -9.81f;
```
And put this into the Update function.
```ruby
velocity.y += gravity * Time.deltaTime;
```
To add velocity to the player we need to reference the 'CharacterContoller' again.
```ruby
controller.Move(velocity * Time.deltaTime);
```
This should now allow us to fall.
However our velocity will constantly build up as we have to reset it after we touch the ground. We will add a ``` groundCheck ``` to do this.

# 5. Add Ground Check
Add an empty object to the Player object and name it ``` groundCheck ```. Move the empty object to the bottom of the Player, as seen below.

<img width="198" alt="image" src="https://github.com/dixonm25/GpProject1/assets/146852548/fed55794-391a-432e-80af-1c55c0bb2ee4">

This will be used to check if the Player is standing on anything.

After that we have to make a reference for the groundCheck, set a radius for the sphere, and set a LayerMask to check what objects the sphere will check for.
```ruby
public Transform groundCheck;
public float groundDistance = 0.4f;
public LayerMask groundMask; 
```
Next we have to store if we are grounded or not with a boolean variable.
```ruby
bool isGrounded;
```
Now inside the Update function we will check if we are grounded with a physics check

```ruby
isGrounded = Physics.CheckSphere(groundCheck.position, groundDistance, groundMask);

if (isGrounded && velocity.y < 0) 
{
velocity.y = -2f;
}
```
This will allow us to check if we are grounded and also reset the velocity when the player has reached the ground. The reason we use "-2f" instead of 0f is because it might register the reset before we fully touch the ground.

# 6. Add jumping
Now that we have added gravity, velocity and a groundCheck we can add a jump mechanic. All we have to do for us to jump is to check if we are currently grounded and if we are pressing the jump button.

```ruby
public float jumpHeight = 3f;
```
This gives a height for us to jump at.

```ruby
if(Input.GetButtonDown("Jump") && isGrounded) 
{
    velocity.y = Mathf.Sqrt(jumpHeight * -2f * gravity);
}
```
This checks to see if we are grounded and also if we are pressing the jump button. If these conditions are met we jump.

Here is what the gameplay should look like:

https://github.com/dixonm25/Platform-game/assets/146852548/b917e831-5abf-4435-a656-76b39ff5a31b


