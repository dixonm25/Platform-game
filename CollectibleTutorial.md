# Collectible Tutorial
This tutorial will show you how to add collectibles. Code was taken from - https://www.youtube.com/watch?v=jJonSISrTqQ

# 1. Add a coin object
Create and scale an object that will represent your coin.
Drag this object into the project panel to make it a prefab. This will make it so that when you duplicate the object it will still have the components and values that was added to the original.
Make sure that in the capsule collider for the object the "Is Trigger" box is ticked. This allows us to trigger a special event when the player interacts with it.

# 2. Add a script called "Collectible"
First in the Update function add the following code.
```ruby
transform.localRotation = Quaternion.Euler(90f, Time.time * 100f, 0);
```
This will make the coins rotate 100 degrees per second.

# 3. Add a coin counter
Now back in unity we will add a counter to see how many coins we have picked up. 
To do this we will add a canvas object, this can be found under the "UI" tab as shown below:

<img width="355" alt="image" src="https://github.com/dixonm25/GpProject1/assets/146852548/378d7387-a746-49f1-81d4-8ee4268f3ac5">

After adding the canvas set the scale mode to "Scale with screen size" so that it will stay in the same position no matter what sized screen the game is played on.

Now add a text object to the canvas and call it "Count" which will be used to display the text which will be updated while playing.

# 4. Add a new script called "CollectibleCount"
In the text object add a new script called "CollectibleCount". In this script we will keep track of the number of coins we have collected and also see the maximum number of coins that we can collect on the map.

Back in the "Collectible" script we will add:
```ruby
public static event Action OnCollected;
```
For this to work we have to add ``` using System ; ``` to the top of the code so that the Action function can be used.

After that add:
```ruby
void OnTriggerEnter(Collider other) 
    {
        if(other.CompareTag("Player"))
        {
            OnCollected?.Invoke();
            Destroy(gameObject);
        }
    
    }
```
This will be used to check when something collides with the collectible. It will check whether the object that collided with the coin is a "Player" object, if it is it will invoke the event that will be linked with the coin object and destroy the coin so that it can no longer be interacted with.

# 5. Finish coin counter
Open the "CollectibleCount" script and add:
```ruby
TMPro.TMP_Text text;
    int count;

    void Awake()
    {
        text = GetComponent<TMPro.TMP_Text>();
    }
```
This is used to reference the text component.

```ruby
void OnCollectibleCollected()
    {
        count++;
        UpdateCount();
    }
```
This will update the count when we collect a coin and call the "UpdateCount()" so that the text updates.

```ruby
void OnEnable() => Collectible.OnCollected += OnCollectibleCollected;
void OnDisable() => Collectible.OnCollected -= OnCollectibleCollected;
```
"OnEnable" will register with the "Collectible.OnCollected" event and "OnDisable" will unregister it.

In the "Collectible" script add this:
```ruby
public static int total;

void Awake() => total++;
```
This will make it so that the total is set at 0 when the game starts.

Back in the "CollectibleCount" script add this so that the text displays the total number of coins the player has to collect with the amount they have collected so far.
```ruby
void UpdateCount()
{
    text.text = $"{count} / {Collectible.total} coins";
}
```
This will also update the the text during the game when a coin is collected.

Here is what the gameplay should look like: 

https://github.com/dixonm25/Platform-game/assets/146852548/d478f92f-2d17-48e2-8ca3-a6e93d155798


