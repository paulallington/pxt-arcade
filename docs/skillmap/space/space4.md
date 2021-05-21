# Fuel Up!

## Introduction @showdialog

Time to refuel! 

In this tutorial we'll add a fuel bar to your spaceship
that depletes as you travel. 

Make sure to catch the powerups to keep your
ship from breaking down!

![Fuel Up!](/static/skillmap/space/spacet4.gif "Those aren't tacos!")


## Step 1
😨 The starter code is taking up a lot of room! 
Don't worry, the Arcade workspace will expand for you. Just scroll up and
over (or down and over) to keep building.

---


► Take a peek into the new ``||statusbars:Status Bars||`` category.
You'll find ``||variables:set [statusbar] to create status bar sprite width [20] height [4] kind [Health]||``.
Drag one to the end of the ``||loops:on start||`` container.

► To keep track of how much *gas* is left, set the argument for 
**statusbar** kind to **Energy**.

---

```block
let statusbar = statusbars.create(20, 4, StatusBarKind.Energy)
```

## Step 2

If we want the status bar to show the details of **mySprite**, we'll need to link the two together.

---

► Drop ``||statusbars:attach [statusbar] to [mySprite] ⊕||`` 
into **the end** of the ``||loops:on start||`` container.

► Click **⊕** on the new block to reveal options
 to change the position of the status bar in relation to **mySprite**. 
 Can you figure out how to get the bar to show up *below* your ship?  


```block
let statusbar = statusbars.create(20, 4, StatusBarKind.Energy)
// @highlight
statusbar.attachToSprite(mySprite, -30, 0)
```

## Step 3

**⏰ The longer you're in the air, the more fuel you use ⏰ **

Here's how to make the fuel go down as time passes. 

---

► Drag an ``||game:on game update every [500] ms||`` container into the 
workspace. Adjust the time argument to **300 ms**.

► Drop ``||statusbars:change [statusbar] [value] by [0]||``
into the empty **game update** container.

► Change the amount the status bar changes from **0** to **-1**. 

---

**Tip:** Remember this step later. If the fuel runs out too fast in 
gameplay, you can come back and adjust these blocks.


```blocks
let statusbar: StatusBarSprite = null
game.onUpdateInterval(300, function () {
    statusbar.value += -1
})
```

## Step 4

**⛽ Time to refuel ⛽**

The code for dropping fuel is a lot like the code for dropping enemies. 
For a refresher on how things work, find the **myEnemy** blocks in the
workspace and use them as a guide.

---

► Drag a _new_  ``||game:on game update every [500] ms||`` container 
into the workspace and change the interval to **5 seconds (5000 ms)**.

► Snap a new
``||variables:set [projectile2] to||`` ``||sprites:projectile [ ] from side with vx [50] vy [50]||``
block inside the newest **on game update** container.

► Click ``||variables:[projectile2]||`` and rename the sprite ``||variables:[myFuel]||``.


```blocks
game.onUpdateInterval(5000, function () {
    let myFuel = sprites.createProjectileFromSide(img`
. . . . 
. . . . 
. . . . 
. . . . 
`, 50, 50)
})
```

## Step 5

► Click on the grey square and toggle to **My Assets** to choose the **Fuel** sprite.

► Play with the **vx** and **vy** arguments of the fuel until it's falling
straight down at a decent speed.  



```blocks
game.onUpdateInterval(5000, function () {
    let myFuel = sprites.createProjectileFromSide(assets.image`Fuel`, 0, 100)
})
```

## Step 6

Just like with the enemies, we'll want the fuel to drop from a random position
across the top of the screen. 

---

► Connect a ``||sprites:set [mySprite] [x] to [0]||`` block at the 
bottom of the ``||game:on game update every [5000] ms||`` container.  

► To make sure we're acting on the right sprites, use the dropdown in the 
new block to change ``||variables:mySprite||`` to ``||variables:myFuel||``.


```blocks
game.onUpdateInterval(5000, function () {
    let myFuel = sprites.createProjectileFromSide(assets.image`Fuel`, 0, 100)
    myFuel.x = 0
})
```

# Step 7

► To set a random [__*x*__](#setX "horizontal location") 
for the fuel, grab a 
``||Math:pick random [0] to [10]||`` block
and connect it to replace the **0** argument in the 
``||sprites:set [mySprite] [x] to [0]||`` block.

► Update the minimum argument of the ``||Math:pick random [0] to [10]||`` block to **5** and the
maximum argument to **155**. 

---


```blocks
game.onUpdateInterval(5000, function () {
    let myFuel = sprites.createProjectileFromSide(assets.image`Fuel`, 0, 100)
    myFuel.x = randint(5, 155)
})
```


## Step 8

Now we need to put our **myFuel** sprite into the _Gas_ class.

---

► Snap a ``||sprites:set [mySprite] kind to [Player]||`` block 
into the bottom of the newest **on game update** container.

► Change ``||variables:mySprite||`` to ``||variables:myFuel||``. 

► Click ``||sprites:Player||`` to get the menu, then choose
``||sprites:Add a new kind...||`` and create the type **Gas**.   


```blocks
namespace SpriteKind {
    export const Gas = SpriteKind.create()
}

game.onUpdateInterval(5000, function () {
    let myFuel = sprites.createProjectileFromSide(assets.image`Fuel`, 0, 100)
    myFuel.x = randint(5, 155)
    myFuel.setKind(SpriteKind.Gas)
})
```


## Step 9
When your ship overlaps fuel, you'll want the gas to disappear as the tank refills.

---

► Drag an ``||sprites:on [sprite] of kind [Player] overlaps [othersprite] of kind [Player]||`` 
container into the workspace. 

► Change the last argument from ``||sprites:Player||`` to ``||sprites:Gas||``.  


  
```blocks
namespace SpriteKind {
    export const Gas = SpriteKind.create()
}

let statusbar: StatusBarSprite = null
sprites.onOverlap(SpriteKind.Player, SpriteKind.Gas, function (sprite, otherSprite) {

})
```

## Step 10

► To refill the status bar after grabbing fuel, snag a ``||statusbars:set [statusbar] [value] to [0]||`` block 
and snap it in to your newest **overlaps** container.  

► Change the value from **0** to **100**.

► Finally, make sure the used fuel disappears by snapping a ``||sprites:destroy [mySprite] ⊕||`` block 
into the bottom of the same **overlaps** container and replacing
``||variables:mySprite||`` with ``||variables:otherSprite||``

![Grabbing variable from block](/static/skillmap/space/give-var.gif "So that's how you do that!")  

  
```blocks
namespace SpriteKind {
    export const Gas = SpriteKind.create()
}

let statusbar: StatusBarSprite = null
sprites.onOverlap(SpriteKind.Player, SpriteKind.Gas, function (sprite, otherSprite) {
    statusbar.value = 100
    otherSprite.destroy()
})
```

## Step 11
**🌌 If you run out of fuel, you'll be marooned in space! 🌌**

The threat is real.

---

► To add consequences for an empty status bar, drag a 
``||statusbars:on status bar kind [Health] zero [status]||`` 
container into the workspace.

► Change the status bar kind to **Energy**. 

► Snap a ``||game:game over <LOSE>||`` block inside as the ultimate fate.


```blocks
statusbars.onZero(StatusBarKind.Energy, function (status) {
    game.over(false)
})
```


## Finale

**And that's it!** 

Click **Finish** to return to the main page where you can add this game to your gallery and share with family & friends.

Once your game is in your gallery, you can
experiment with all of the blocks in the toolbox and find many other
exciting and special ways to customize your adventure.  





```package
arcade-background-scroll=github:microsoft/arcade-background-scroll/
pxt-status-bar=github:jwunderl/pxt-status-bar
```


```template

controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    projectile = sprites.createProjectileFromSprite(assets.image`Dart1`, mySprite, 0, -150)
    projectile.startEffect(effects.ashes)
})

sprites.onOverlap(SpriteKind.Projectile, SpriteKind.Enemy, function (sprite, otherSprite) {
    sprite.destroy(effects.fire, 100)
    otherSprite.destroy()
    info.changeScoreBy(1)
})
sprites.onOverlap(SpriteKind.Player, SpriteKind.Enemy, function (sprite, otherSprite) {
    info.changeLifeBy(-1)
    otherSprite.destroy(effects.disintegrate, 200)
})
let myEnemy: Sprite = null
let projectile: Sprite = null
let mySprite: Sprite = null
scene.setBackgroundImage(assets.image`Galaxy`)
scroller.scrollBackgroundWithSpeed(0, 10)
mySprite = sprites.create(assets.image`Rocket`, SpriteKind.Player)
controller.moveSprite(mySprite, 100, 100)
mySprite.setFlag(SpriteFlag.StayInScreen, true)


game.onUpdateInterval(2000, function () {
    myEnemy = sprites.createProjectileFromSide(assets.image`Spider`, 0, 50)
    myEnemy.x = randint(5, 155)
    myEnemy.setKind(SpriteKind.Enemy)
})
```

```ghost
namespace SpriteKind {
    export const Gas = SpriteKind.create()
}
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    projectile = sprites.createProjectileFromSprite(assets.image`Dart1`, mySprite, 0, -150)
    projectile.startEffect(effects.ashes)
})
sprites.onOverlap(SpriteKind.Player, SpriteKind.Gas, function (sprite, otherSprite) {
    statusbar.value = 100
    otherSprite.destroy()
})
statusbars.onZero(StatusBarKind.Energy, function (status) {
    game.over(false)
})
sprites.onOverlap(SpriteKind.Projectile, SpriteKind.Enemy, function (sprite, otherSprite) {
    sprite.destroy(effects.fire, 100)
    otherSprite.destroy()
    info.changeScoreBy(1)
})
sprites.onOverlap(SpriteKind.Player, SpriteKind.Enemy, function (sprite, otherSprite) {
    info.changeLifeBy(-1)
    otherSprite.destroy(effects.disintegrate, 500)
})
let myEnemy: Sprite = null
let myFuel: Sprite = null
let projectile: Sprite = null
let statusbar: StatusBarSprite = null
let mySprite: Sprite = null
scene.setBackgroundImage(assets.image`Galaxy`)
scroller.scrollBackgroundWithSpeed(0, 10)
mySprite = sprites.create(assets.image`Rocket`, SpriteKind.Player)
controller.moveSprite(mySprite, 100, 100)
mySprite.setFlag(SpriteFlag.StayInScreen, true)
statusbar = statusbars.create(20, 4, StatusBarKind.Energy)
statusbar.attachToSprite(mySprite, -30, 0)
mySprite.y = 100
game.onUpdateInterval(5000, function () {
    myFuel = sprites.createProjectileFromSide(assets.image`Fuel`, 0, 100)
    myFuel.x = randint(5, 155)
    myFuel.setKind(SpriteKind.Gas)
})
game.onUpdateInterval(2000, function () {
    myEnemy = sprites.createProjectileFromSide(assets.image`Spider`, 0, 50)
    myEnemy.x = randint(5, 155)
    myEnemy.setKind(SpriteKind.Enemy)
})
game.onUpdateInterval(300, function () {
    statusbar.value += -1
})
```

```assetjson
{
  "README.md": " ",
  "assets.json": "",
  "images.g.jres": "{\n    \"image6\": {\n        \"data\": \"hwQMABAAAAAAACBAAAAAAAAAAFJEAAAAAAAAQlQEAAAgAPhCRFQEAJDyGEQiIlIAAvlRVVVVVVYC+V9VVVVVVpDyWEQiIlIAIAD4QkRUBAAAAABCVAQAAAAAAFJEAAAAAAAgQAAAAAA=\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"Stealth\"\n    },\n    \"image2\": {\n        \"data\": \"hwQQABQAAAAAAABAmZaZlgAAAAAAAABAZmZm1gAAAAAAAAAAYNbd3QAAAAAAAAAAYGYAAAAAAAAAAAAAAGAGAAAAAAAAAGlmZmZmZgBpAAAAlmaZZplpaWBmAACZlmaZmWmZmZZpAACZZmZmZmZmZpZmAAAAlmZmZmZmZpBpAAAAAGZmZmZmZgBpAAAAAAAAAAAGAAAAAAAAAAAAAGYGAAAAAAAAAAAAYNbd3QAAAAAAAABAlpaZ1gAAAAAAAABAZmZmZgAAAAA=\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"Blue Rocket\"\n    },\n    \"xlh}4fdz:|FpuFr9_v_b\": {\n        \"data\": \"hwQNAA0AAAAAsLsAuwsAAACbs7BVCwAAABu5sFELAAAAsLu7uwsAAAAAsFm7uwsAAACbPbVbCwCwu5s5tbsAABu5G5G7uwAAsAuwuzs5CwAAALsLm5ELAACwmQs7OQsAALCRCwC7AAAAALsAAAAAAA==\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"Fuel\"\n    },\n    \"image21\": {\n        \"data\": \"hwQDAAQAAABQKgAApQIAAFAqAAA=\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"Dart1\"\n    },\n    \"image1\": {\n        \"data\": \"hwQGAAYAAAAAdwAAAHFwABFxBwARcQcAAHFwAAB3AAA=\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"Dart2\"\n    },\n    \"image3\": {\n        \"data\": \"hwQOABMAAAAAAAAAAACZDQAAAAAAAAAAAJlmAAAAAAAAAAAAmfb/AAAAAAAAAACR9v8PAEAAAAAAGRFh////AAQAAACQERYRERHxSlAAAAAZYcwRERHxWkQAAAAZocwRERHxWlQEAACQERoRERHxSlAAAAAAGRGh////AAQAAAAAAACR+v8PAEAAAAAAAAAAmfr/AAAAAAAAAAAAAJmqAAAAAAAAAAAAAACZCQAAAAA=\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"Rocket\"\n    },\n    \"image7\": {\n        \"data\": \"hwQVAAwAAAAAAAAuAAAAAAAAAORSAAAAAABARC4AAAAAAEBFJAUAAAAAQETkAgAAAJmZREQCAACQmZhJRQIAAJmZiUlEAgAAmZmJSUQCAACZmZmZRAIAAJmZmZlEAgAAGZGZmUQCAAAZkZlJRAIAAJkRmUlEAgAAkBmRSUUCAAAAmZlERAIAAAAAQETkAgAAAABARSQFAAAAAEBELgAAAAAAAORSAAAAAAAALgAAAAA=\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"UFO\"\n    },\n    \"image4\": {\n        \"data\": \"hwSgAHgAAADMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzLzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzszMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMvMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyMyMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyMyMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyIiMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzIyIiMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzIyIaMzMzMzMzMzMzMzMzMzMzMzMzMzMzszMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzIiIZszMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMjIiIZszMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMjIiIZszMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMiIiIZszMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzPz//4+IZszMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzM/P///4+I9szMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMvMzMzMzMzMzMzMzMzMzM//////+I9s/MzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMz8+/v///+I9v/MzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMz8+/u///+I9v/MzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMy/vPv//4+I9v/PzMzMzMzMzMzMzLzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMy/u7u/+4+I9v/PzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMy/u7u7u4uI9v/PzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMy/u7u7u4xm9v/PzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMvMzMzMzMzMzMzMzMzMzMzMzMzMzMy/u7u7u4hm///PzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzszMzMy/u7u7u4hm+/vPzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMy/u7u7u2i2vP/PzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMy/u7u7i2a2u//PzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMy/u7u7i2bIy/vPzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMz8u7u7a2bIzPvMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMz8v7u7aGa7vP/MzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMuLu7aIbLu8/MzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzM+L+LZoa7/8zMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMiPhvZvj/zMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyMiIhoZsjMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyMiIhmhszMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyMiIhmxszMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyMiGjGzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyMiGbGzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyMiMbMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMy8zMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyMaMbMzMzMzMzMzMzMzMvMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMxsZszMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMvMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMy8zMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyIiIjMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMjIj/j4iIzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzM+P////+PyMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyM////////iMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzLzMzMzMzMzMzMzMzMzMzMzMz4//+Pj4//j8jMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMz4+P///////8jMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMz/+v//+P+P/8jMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyP+q///////8jMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyI+qb4j4j//8jMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyouvr/+G9m/8jMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyIqqqqpoZo9sjMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyIqquqaoaIhsjMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyIqqqqaoaIxsjMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMz4aGqqqmarqsjMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyM+Kqqq6qmiszMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMiPpqqqaqyszMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzOzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMjI+KqqivzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzszMzMzMzMzMzMzIiIiIjIzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzLzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMy8zMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzszMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMvMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzLzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzOzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMz////MzMzMzMzMzMzMzMzMzMzMzMzMzMvMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzPz/////zMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzM/P///////8zMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzM/////////8/MzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMz8///////////MzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMz8///////////MzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMz2///////////PzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzLzMzMzMzMzMzMzMzMzMz2////Zv/////PzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMxm9vav9q//9v/PzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMxoZv9m9vb2b//GzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyGZmZmZvZvZv/PzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMxoZmZmZmZmZvbPzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyIZmZmZmZmiGbPzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyIZmaGZoZmZmbIzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyPhmZmb2ZqaGbIzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyMhmZmpmZmZobMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMyMiIZoZmZmhobMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMiGhmhohoiMjMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMjIiIaGZmiMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzIiIyIiIzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMxsiMjMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMy8zMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzLzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzIzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzOzMzMzMzMzMzMzMzMzMzMzMzMvMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzM7MzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzLzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMw=\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"Galaxy\"\n    },\n    \"image17\": {\n        \"data\": \"hwQPABIAAADyAgAAAAAAAAAAAADy/wIAAAAAAAAAAAAv/yIAAAAAAAAAAADw8i8CAAAAAAAAAAAAL/8i8v8CAAAAAAAA8PIv8rv/AgAAAAD/Ly8i8vv//wIAAAD/IiLyL/////8AAAD/Ly8i8vv//wIAAAAA8PIv8rv/AgAAAAAAL/8i8v8CAAAAAADw8i8CAAAAAAAAAAAv/yIAAAAAAAAAAADy/wIAAAAAAAAAAADyAgAAAAAAAAAAAAA=\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"Spider\"\n    },\n    \"*\": {\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"dataEncoding\": \"base64\",\n        \"namespace\": \"myImages\"\n    }\n}",
  "images.g.ts": "// Auto-generated code. Do not edit.\nnamespace myImages {\n\n    helpers._registerFactory(\"image\", function(name: string) {\n        switch(helpers.stringTrim(name)) {\n            case \"image6\":\n            case \"Stealth\":return img`\n. . . . . 2 2 . . . . . \n. . . 2 9 . . 9 2 . . . \n. . . . 2 9 9 2 . . . . \n. . . . f f f f . . . . \n. . . 8 8 1 f 8 8 . . . \n2 . . f 1 5 5 5 f . . 2 \n. 2 2 2 4 5 5 4 2 2 2 . \n4 5 4 4 4 5 5 4 4 4 5 4 \n. 4 4 4 2 5 5 2 4 4 4 . \n. 4 5 4 2 5 5 2 4 5 4 . \n. . 4 4 2 5 5 2 4 4 . . \n. . . 5 2 5 5 2 5 . . . \n. . . 4 2 5 5 2 4 . . . \n. . . . 5 5 5 5 . . . . \n. . . . . 6 6 . . . . . \n. . . . . 5 5 . . . . . \n`;\n            case \"image2\":\n            case \"Blue Rocket\":return img`\n.......99.......\n.......99.......\n......6666......\n......9969......\n.....966666.....\n.....666666.....\n.....699666.....\n44...699666...44\n96...669666...66\n9666.669666..696\n6666.699666.6666\n96d66696666.6d96\n96d.669966666d96\n96d..669666..d96\n66d..699666..d66\n9dd..669666..dd6\n.......66.......\n......6999......\n.....969699.....\n.....666666.....\n`;\n            case \"xlh}4fdz:|FpuFr9_v_b\":\n            case \"Fuel\":return img`\n. . . . . . . b . . . . . \n. . . . . . b 1 b . . . . \n. b b . . . b 9 b . . . . \nb 9 1 b . . b b . . b b . \nb 3 9 b . b b b . b 9 1 b \nb b b b b 9 9 1 b b 9 9 b \n. . . b 9 d 9 1 b b b b . \n. b b b 5 3 3 9 b . . . . \nb 5 1 b b 5 5 b b b b . . \nb 5 5 b b b b b 3 9 3 . . \nb b b b b b b b 9 1 9 b . \n. . . . b 5 b b 3 9 3 b . \n. . . . b b . . b b b . . \n`;\n            case \"image21\":\n            case \"Dart1\":return img`\n. 5 . \n5 a 5 \na 2 a \n2 . 2 \n`;\n            case \"image1\":\n            case \"Dart2\":return img`\n. . 1 1 . . \n. . 1 1 . . \n7 1 1 1 1 7 \n7 7 7 7 7 7 \n. . 7 7 . . \n. 7 . . 7 . \n`;\n            case \"image3\":\n            case \"Rocket\":return img`\n. . . . . . 9 9 . . . . . . \n. . . . . 9 1 1 9 . . . . . \n. . . . 9 1 1 1 1 9 . . . . \n. . . . 1 1 6 a 1 1 . . . . \n. . . . 1 6 c c a 1 . . . . \n. . . . 1 1 c c 1 1 . . . . \n. . . 1 1 1 1 1 1 1 1 . . . \n. . . 9 6 1 1 1 1 a 9 . . . \n. . 9 6 f 1 1 1 1 f a 9 . . \n. . 9 f f 1 1 1 1 f f 9 . . \n. 9 6 f f 1 1 1 1 f f a 9 . \n. 9 f f f 1 1 1 1 f f f 9 . \n9 6 f f f 1 1 1 1 f f f a 9 \n9 6 f . f f f f f f . f a 9 \nd . . . . a a a a . . . . 9 \n. . . . . 4 5 5 4 . . . . . \n. . . . 4 . 4 4 . 4 . . . . \n. . . 4 . 5 4 5 5 . 4 . . . \n. . . . . . . 4 . . . . . . \n`;\n            case \"image7\":\n            case \"UFO\":return img`\n. . . . . . . 9 9 9 9 9 9 9 . . . . . . . \n. . . . . . 9 9 9 9 9 1 1 9 9 . . . . . . \n. . . . . 9 9 9 9 9 9 1 1 1 9 9 . . . . . \n. . . . . 9 9 9 9 9 9 9 9 1 1 9 . . . . . \n. . . . . 9 8 9 9 9 9 9 9 9 1 9 . . . . . \n. . 4 4 4 9 9 8 8 9 9 9 9 9 9 9 4 4 4 . . \ne 4 4 5 4 4 9 9 9 9 9 9 9 9 9 4 4 5 4 4 e \n2 e 4 4 4 4 4 4 4 9 9 9 4 4 4 4 4 4 4 e 2 \n. 2 e 4 4 4 5 4 4 4 4 4 4 4 5 4 4 4 e 2 . \n. 5 2 2 e 4 4 4 4 4 4 4 4 4 4 4 e 2 2 5 . \n. . . 5 2 2 2 2 2 2 2 2 2 2 2 2 2 5 . . . \n. . . . . . . . . . . . . . . . . . . . . \n`;\n            case \"image4\":\n            case \"Galaxy\":return img`\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccecccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccbccccccccccccccccccfffffffffcccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccffbbbbbbbbbffccc88888886ccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccccccccccccccfbbcbbbbbbbbbf88888888886ccccccccccccccccccccccccccccccccccccccccccccceccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccffffbbbbbbbbbbbbf888888866ccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccffbbbbbbbbbbbbbbf88888666cccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccccccccccccffffffbbbbbbbbbbbbf88866cccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccccccccccccfffffffbbbbbbbbbbbf86666ccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccbccccccccccccccccccccccccccccccccc\nccccccccccccccccccccffffbfbbbbbbbbbbb86666ccccccccccccccbccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccccccccccc8ffffffbbbbbbbbb8866666cccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccccccccc888fffffffbbbbb886666668ccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccc8888fffffffbc88866666688cccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccc88888888fff8888886666688fccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccccc8888888888888886666688bbbfccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccc88888888888888888666bbccbcbfccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccc888886666666666666fbcbbccbfcccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccccc88666666ffffffffffbbccbbfcccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccccccccccccccffffffffbffbbffccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccfffffffffffffccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccbccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccfffffffffcccccccccccccccccccccccccccccccccccccccceccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccb8ccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccbcccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccbcccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc66686888fcccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccffff668688888cccccccccccccccccccccccc\nccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccfffff6666666688ccccccccccccccccccccccc\nccccccccccbccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccfffffff6666688888cccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccffffff6f6666666888ccccccccccccccccccccc\ncccccccccccccccccccccccccbccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccfffffffff6666668688ccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccfffffffff66666668688ccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccffffffffa666686666886cccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccbcccccccccccccccccccccccccccfffffff6666666f666888cccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccfffffff6ff66666a686c8cccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccfffffffff666666668688cccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccffffffffaff668666868ccccccccccccccccccccc\nccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccffffffff6f666a66868cccccccccccccccccbccc\nccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccfffffffff6666666668ccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccffffff6f668686688cccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccfffffff6668666888cccccccccccccccccccccc\nccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccffffffff6666668ccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccffffffff66688cccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccfff6fff88cccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccecccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccccccceccccccccccccccccccccccccccccbccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccecccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc88ff888888cccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc8fff88a888f8ccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc8ff8aaaaaaa888cccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc8fffffffbaaa6f88ccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccccccccccccccccccccbcccccccccccccccccccccccccccccc8ffffff6aabaaaaf8ccccccccccccccccccbccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc8fffffaafaaa6af88cccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccc8fffffff8faaaaaaa8cccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccc8fff8fffffaaaaa688cccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccc8fffff8ff86aaabaa8cccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccc88ff8fff8fa66aaaa8cccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccc88ffffff8f6666a688cccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccc88ff8fff868886aaa8cccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc8fffffff6888b6af8cccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc88fff8ff6688aaaacccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccbccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc88ffffff666aaacccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc88ffffff8ca8ccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc8888888888cccccccccccccccccccccccccccccccccccccccccccccccbcccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccecccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccbcccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccccccccccccccccccccccccccccccccccccccccccbccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\nccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccbccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccbcccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccceccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccecccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\ncccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc\n`;\n            case \"image17\":\n            case \"Spider\":return img`\n2 2 f . . . f f f . . . f 2 2 \nf f 2 f . . f f f . . f 2 f f \n2 f f 2 f . f 2 f . f 2 f f 2 \n. f f f 2 f 2 2 2 f 2 f f f . \n. 2 2 f f 2 f 2 f 2 f f 2 2 . \n. . 2 2 f f 2 2 2 f f 2 2 . . \n. . . 2 2 f 2 2 2 f 2 2 . . . \n. . . . 2 2 2 f 2 2 2 . . . . \n. . . . 2 2 2 f 2 2 2 . . . . \n. . . . f f f 2 f f f . . . . \n. . . . f b b f b b f . . . . \n. . . . f b f f f b f . . . . \n. . . . 2 f f f f f 2 . . . . \n. . . . . f f f f f . . . . . \n. . . . . 2 f f f 2 . . . . . \n. . . . . . f f f . . . . . . \n. . . . . . 2 f 2 . . . . . . \n. . . . . . . f . . . . . . . \n`;\n        }\n        return null;\n    })\n\n    helpers._registerFactory(\"animation\", function(name: string) {\n        switch(helpers.stringTrim(name)) {\n\n        }\n        return null;\n    })\n\n}\n// Auto-generated code. Do not edit.\n",
  "main.blocks": "<xml xmlns=\"https://developers.google.com/blockly/xml\"><variables><variable type=\"KIND_StatusBarKind\" id=\"/K%{SpW+hL@|DfMDB(`;\">Health</variable><variable type=\"KIND_StatusBarKind\" id=\"SWldc^f:mw!,xwv@WL?p\">Energy</variable><variable type=\"KIND_StatusBarKind\" id=\"g3Cl-BCte^-mcd)xc*q^\">Magic</variable><variable type=\"KIND_StatusBarKind\" id=\"MhC?k3TZTR8usQHnzI-S\">EnemyHealth</variable><variable id=\"5.(o@)4??y0/k{XK-!y[\">projectile</variable><variable id=\"R7dik,CiE;B047o!sYUb\">mySprite</variable><variable id=\"h1[R_8P)$La{DBVGu]DJ\">myEnemy</variable><variable id=\"0EIjmPrHvl2fFOH-}hD5\">statusbar</variable><variable id=\"T}Ip%yy]AU9bxdT0UDp1\">myFuel</variable><variable id=\"b|;7Q4ozqLRbCLM6lKLe\">projectile2</variable><variable type=\"KIND_SpriteKind\" id=\"I5@$XtX9_FL^Y@!S(vgQ\">Player</variable><variable type=\"KIND_SpriteKind\" id=\"UR$=z1+|70iPrvX+I!($\">Projectile</variable><variable type=\"KIND_SpriteKind\" id=\"0zck@]Ye`{v[!uv;EmA%\">Food</variable><variable type=\"KIND_SpriteKind\" id=\"BAZbTj.O~v5V3vIT`5M5\">Enemy</variable><variable type=\"KIND_SpriteKind\" id=\"w@::)0lsX-)RjzLOGEpv\">StatusBar</variable><variable type=\"KIND_SpriteKind\" id=\"%9)/),)nyhcn(Cy:;g~}\">Gas</variable></variables><block type=\"pxt-on-start\" x=\"0\" y=\"0\"><statement name=\"HANDLER\"><block type=\"gamesetbackgroundimage\"><value name=\"img\"><shadow type=\"background_image_picker\"><field name=\"img\">assets.image`Galaxy`</field><data>{\"commentRefs\":[],\"fieldData\":{\"img\":\"myImages.image4\"}}</data></shadow></value><next><block type=\"variables_set\"><field name=\"VAR\" id=\"R7dik,CiE;B047o!sYUb\">mySprite</field><value name=\"VALUE\"><shadow type=\"math_number\"><field name=\"NUM\">0</field></shadow><block type=\"spritescreate\"><value name=\"img\"><shadow type=\"screen_image_picker\"><field name=\"img\">assets.image`Rocket`</field><data>{\"commentRefs\":[],\"fieldData\":{\"img\":\"myImages.image3\"}}</data></shadow></value><value name=\"kind\"><shadow type=\"spritekind\"><field name=\"MEMBER\">Player</field></shadow></value></block></value><next><block type=\"game_control_sprite\"><mutation xmlns=\"http://www.w3.org/1999/xhtml\" _expanded=\"2\" _input_init=\"true\"></mutation><value name=\"sprite\"><shadow type=\"variables_get\"><field name=\"VAR\" id=\"R7dik,CiE;B047o!sYUb\">mySprite</field></shadow></value><value name=\"vx\"><shadow type=\"math_number\"><field name=\"NUM\">100</field></shadow></value><value name=\"vy\"><shadow type=\"math_number\"><field name=\"NUM\">100</field></shadow></value><next><block type=\"spritesetsetflag\"><field name=\"flag\">SpriteFlag.StayInScreen</field><value name=\"sprite\"><block type=\"variables_get\"><field name=\"VAR\" id=\"R7dik,CiE;B047o!sYUb\">mySprite</field></block></value><value name=\"on\"><shadow type=\"toggleOnOff\"><field name=\"on\">true</field></shadow></value><next><block type=\"variables_set\"><field name=\"VAR\" id=\"0EIjmPrHvl2fFOH-}hD5\">statusbar</field><value name=\"VALUE\"><shadow xmlns=\"http://www.w3.org/1999/xhtml\" type=\"math_number\"><field name=\"NUM\">0</field></shadow><block type=\"statusbars_create\"><value name=\"width\"><shadow type=\"math_number\"><field name=\"NUM\">20</field></shadow></value><value name=\"height\"><shadow type=\"math_number\"><field name=\"NUM\">4</field></shadow></value><value name=\"kind\"><shadow type=\"statusbars_kind\"><field name=\"MEMBER\">Energy</field></shadow></value></block></value><next><block type=\"statusbars_attachToSprite\"><mutation xmlns=\"http://www.w3.org/1999/xhtml\" _expanded=\"2\" _input_init=\"true\"></mutation><value name=\"this\"><block type=\"variables_get\"><field name=\"VAR\" id=\"0EIjmPrHvl2fFOH-}hD5\">statusbar</field></block></value><value name=\"toFollow\"><block type=\"variables_get\"><field name=\"VAR\" id=\"R7dik,CiE;B047o!sYUb\">mySprite</field></block></value><value name=\"padding\"><shadow type=\"math_number\"><field name=\"NUM\">-30</field></shadow></value><value name=\"offset\"><shadow type=\"math_number\"><field name=\"NUM\">0</field></shadow></value><next><block type=\"Sprite_blockCombine_set\"><field name=\"property\">Sprite.y@set</field><value name=\"mySprite\"><block type=\"variables_get\"><field name=\"VAR\" id=\"R7dik,CiE;B047o!sYUb\">mySprite</field></block></value><value name=\"value\"><shadow type=\"math_number\"><field name=\"NUM\">100</field></shadow></value></block></next></block></next></block></next></block></next></block></next></block></next></block></statement></block><block type=\"gameinterval\" x=\"707\" y=\"70\"><value name=\"period\"><shadow type=\"timePicker\"><field name=\"ms\">2000</field></shadow></value><statement name=\"HANDLER\"><block type=\"variables_set\"><field name=\"VAR\" id=\"h1[R_8P)$La{DBVGu]DJ\">myEnemy</field><value name=\"VALUE\"><shadow type=\"math_number\"><field name=\"NUM\">0</field></shadow><block type=\"spritescreateprojectilefromside\"><value name=\"img\"><shadow type=\"screen_image_picker\"><field name=\"img\">assets.image`Spider`</field><data>{\"commentRefs\":[],\"fieldData\":{\"img\":\"myImages.image17\"}}</data></shadow></value><value name=\"vx\"><shadow type=\"spriteSpeedPicker\"><field name=\"speed\">0</field></shadow></value><value name=\"vy\"><shadow type=\"spriteSpeedPicker\"><field name=\"speed\">50</field></shadow></value></block></value><next><block type=\"Sprite_blockCombine_set\"><field name=\"property\">Sprite.x@set</field><value name=\"mySprite\"><block type=\"variables_get\"><field name=\"VAR\" id=\"h1[R_8P)$La{DBVGu]DJ\">myEnemy</field></block></value><value name=\"value\"><block type=\"device_random\"><value name=\"min\"><shadow type=\"math_number\"><field name=\"NUM\">5</field></shadow></value><value name=\"limit\"><shadow type=\"math_number\"><field name=\"NUM\">155</field></shadow></value></block></value><next><block type=\"spritesetkind\"><value name=\"sprite\"><block type=\"variables_get\"><field name=\"VAR\" id=\"h1[R_8P)$La{DBVGu]DJ\">myEnemy</field></block></value><value name=\"kind\"><shadow type=\"spritekind\"><field name=\"MEMBER\">Enemy</field></shadow></value></block></next></block></next></block></statement></block><block type=\"keyonevent\" x=\"1505\" y=\"70\"><field name=\"button\">controller.A</field><field name=\"event\">ControllerButtonEvent.Pressed</field><statement name=\"HANDLER\"><block type=\"variables_set\"><field name=\"VAR\" id=\"5.(o@)4??y0/k{XK-!y[\">projectile</field><value name=\"VALUE\"><shadow type=\"math_number\"><field name=\"NUM\">0</field></shadow><block type=\"spritescreateprojectilefromsprite\"><value name=\"img\"><shadow type=\"screen_image_picker\"><field name=\"img\">assets.image`Dart1`</field><data>{\"commentRefs\":[],\"fieldData\":{\"img\":\"myImages.image21\"}}</data></shadow></value><value name=\"sprite\"><shadow type=\"variables_get\"><field name=\"VAR\" id=\"R7dik,CiE;B047o!sYUb\">mySprite</field></shadow></value><value name=\"vx\"><shadow type=\"spriteSpeedPicker\"><field name=\"speed\">0</field></shadow></value><value name=\"vy\"><shadow type=\"spriteSpeedPicker\"><field name=\"speed\">-150</field></shadow></value></block></value><next><block type=\"startEffectOnSprite\"><mutation xmlns=\"http://www.w3.org/1999/xhtml\" _expanded=\"0\" _input_init=\"true\"></mutation><field name=\"effect\">effects.ashes</field><value name=\"sprite\"><block type=\"variables_get\"><field name=\"VAR\" id=\"5.(o@)4??y0/k{XK-!y[\">projectile</field></block></value><value name=\"duration\"><shadow type=\"timePicker\"><field name=\"ms\">100</field></shadow></value></block></next></block></statement></block><block type=\"spritesoverlap\" x=\"841\" y=\"405\"><value name=\"HANDLER_DRAG_PARAM_sprite\"><shadow type=\"argument_reporter_custom\"><mutation typename=\"Sprite\"/><field name=\"VALUE\">sprite</field></shadow></value><value name=\"kind\"><shadow type=\"spritekind\"><field name=\"MEMBER\">Player</field></shadow></value><value name=\"HANDLER_DRAG_PARAM_otherSprite\"><shadow type=\"argument_reporter_custom\"><mutation typename=\"Sprite\"/><field name=\"VALUE\">otherSprite</field></shadow></value><value name=\"otherKind\"><shadow type=\"spritekind\"><field name=\"MEMBER\">Enemy</field></shadow></value><statement name=\"HANDLER\"><block type=\"hudChangeLifeBy\"><value name=\"value\"><shadow type=\"math_number\"><field name=\"NUM\">-1</field></shadow></value><next><block type=\"spritedestroy\"><mutation xmlns=\"http://www.w3.org/1999/xhtml\" _expanded=\"2\" _input_init=\"true\"></mutation><field name=\"effect\">effects.disintegrate</field><value name=\"sprite\"><block type=\"argument_reporter_custom\"><mutation typename=\"Sprite\"/><field name=\"VALUE\">otherSprite</field></block></value><value name=\"duration\"><shadow type=\"timePicker\"><field name=\"ms\">500</field></shadow></value></block></next></block></statement></block><block type=\"spritesoverlap\" x=\"30\" y=\"490\"><value name=\"HANDLER_DRAG_PARAM_sprite\"><shadow type=\"argument_reporter_custom\"><mutation typename=\"Sprite\"/><field name=\"VALUE\">sprite</field></shadow></value><value name=\"kind\"><shadow type=\"spritekind\"><field name=\"MEMBER\">Projectile</field></shadow></value><value name=\"HANDLER_DRAG_PARAM_otherSprite\"><shadow type=\"argument_reporter_custom\"><mutation typename=\"Sprite\"/><field name=\"VALUE\">otherSprite</field></shadow></value><value name=\"otherKind\"><shadow type=\"spritekind\"><field name=\"MEMBER\">Enemy</field></shadow></value><statement name=\"HANDLER\"><block type=\"spritedestroy\"><mutation xmlns=\"http://www.w3.org/1999/xhtml\" _expanded=\"2\" _input_init=\"true\"></mutation><field name=\"effect\">effects.fire</field><value name=\"sprite\"><block type=\"argument_reporter_custom\"><mutation typename=\"Sprite\"/><field name=\"VALUE\">sprite</field></block></value><value name=\"duration\"><shadow type=\"timePicker\"><field name=\"ms\">100</field></shadow></value><next><block type=\"spritedestroy\"><mutation xmlns=\"http://www.w3.org/1999/xhtml\" _expanded=\"0\" _input_init=\"true\"></mutation><field name=\"effect\">effects.smiles</field><value name=\"sprite\"><block type=\"argument_reporter_custom\"><mutation typename=\"Sprite\"/><field name=\"VALUE\">otherSprite</field></block></value><value name=\"duration\"><shadow type=\"timePicker\"><field name=\"ms\">500</field></shadow></value><next><block type=\"hudChangeScoreBy\"><value name=\"value\"><shadow type=\"math_number\"><field name=\"NUM\">1</field></shadow></value></block></next></block></next></block></statement></block><block type=\"gameinterval\" x=\"540\" y=\"640\"><value name=\"period\"><shadow type=\"timePicker\"><field name=\"ms\">5000</field></shadow></value><statement name=\"HANDLER\"><block type=\"variables_set\"><field name=\"VAR\" id=\"T}Ip%yy]AU9bxdT0UDp1\">myFuel</field><value name=\"VALUE\"><shadow xmlns=\"http://www.w3.org/1999/xhtml\" type=\"math_number\"><field name=\"NUM\">0</field></shadow><block type=\"spritescreateprojectilefromside\"><value name=\"img\"><shadow type=\"screen_image_picker\"><field name=\"img\">assets.image`Fuel`</field><data>{\"commentRefs\":[],\"fieldData\":{\"img\":\"myImages.xlh}4fdz:|FpuFr9_v_b\"}}</data></shadow></value><value name=\"vx\"><shadow type=\"spriteSpeedPicker\"><field name=\"speed\">0</field></shadow></value><value name=\"vy\"><shadow type=\"spriteSpeedPicker\"><field name=\"speed\">100</field></shadow></value></block></value><next><block type=\"Sprite_blockCombine_set\"><field name=\"property\">Sprite.x@set</field><value name=\"mySprite\"><block type=\"variables_get\"><field name=\"VAR\" id=\"T}Ip%yy]AU9bxdT0UDp1\">myFuel</field></block></value><value name=\"value\"><shadow type=\"math_number\"><field name=\"NUM\">0</field></shadow><block type=\"device_random\"><value name=\"min\"><shadow type=\"math_number\"><field name=\"NUM\">5</field></shadow></value><value name=\"limit\"><shadow type=\"math_number\"><field name=\"NUM\">155</field></shadow></value></block></value><next><block type=\"spritesetkind\"><value name=\"sprite\"><block type=\"variables_get\"><field name=\"VAR\" id=\"T}Ip%yy]AU9bxdT0UDp1\">myFuel</field></block></value><value name=\"kind\"><shadow type=\"spritekind\"><field name=\"MEMBER\">Gas</field></shadow></value></block></next></block></next></block></statement></block><block type=\"gameinterval\" x=\"80\" y=\"740\"><value name=\"period\"><shadow type=\"timePicker\"><field name=\"ms\">300</field></shadow></value><statement name=\"HANDLER\"><block type=\"StatusBarSprite_blockCombine_change\"><field name=\"property\">StatusBarSprite.value@set</field><value name=\"statusbar\"><block type=\"variables_get\"><field name=\"VAR\" id=\"0EIjmPrHvl2fFOH-}hD5\">statusbar</field></block></value><value name=\"value\"><shadow type=\"math_number\"><field name=\"NUM\">-1</field></shadow></value></block></statement></block><block type=\"spritesoverlap\" x=\"540\" y=\"920\"><value name=\"HANDLER_DRAG_PARAM_sprite\"><shadow type=\"argument_reporter_custom\"><mutation typename=\"Sprite\"/><field name=\"VALUE\">sprite</field></shadow></value><value name=\"kind\"><shadow type=\"spritekind\"><field name=\"MEMBER\">Player</field></shadow></value><value name=\"HANDLER_DRAG_PARAM_otherSprite\"><shadow type=\"argument_reporter_custom\"><mutation typename=\"Sprite\"/><field name=\"VALUE\">otherSprite</field></shadow></value><value name=\"otherKind\"><shadow type=\"spritekind\"><field name=\"MEMBER\">Gas</field></shadow></value><statement name=\"HANDLER\"><block type=\"StatusBarSprite_blockCombine_set\"><field name=\"property\">StatusBarSprite.value@set</field><value name=\"statusbar\"><block type=\"variables_get\"><field name=\"VAR\" id=\"0EIjmPrHvl2fFOH-}hD5\">statusbar</field></block></value><value name=\"value\"><shadow type=\"math_number\"><field name=\"NUM\">100</field></shadow></value><next><block type=\"spritedestroy\"><mutation xmlns=\"http://www.w3.org/1999/xhtml\" _expanded=\"0\" _input_init=\"false\"></mutation><value name=\"sprite\"><block type=\"argument_reporter_custom\"><mutation typename=\"Sprite\"/><field name=\"VALUE\">otherSprite</field></block></value></block></next></block></statement></block><block type=\"statusbars_onZero\" x=\"420\" y=\"1100\"><value name=\"kind\"><shadow type=\"statusbars_kind\"><field name=\"MEMBER\">Energy</field></shadow></value><value name=\"HANDLER_DRAG_PARAM_status\"><shadow type=\"argument_reporter_custom\"><mutation typename=\"StatusBarSprite\"/><field name=\"VALUE\">status</field></shadow></value><statement name=\"HANDLER\"><block type=\"gameOver\"><mutation xmlns=\"http://www.w3.org/1999/xhtml\" _expanded=\"0\" _input_init=\"false\"></mutation><value name=\"win\"><shadow type=\"toggleWinLose\"><field name=\"win\">false</field></shadow></value></block></statement></block></xml>",
  "main.ts": "namespace SpriteKind {\n    export const Gas = SpriteKind.create()\n}\ncontroller.A.onEvent(ControllerButtonEvent.Pressed, function () {\n    projectile = sprites.createProjectileFromSprite(assets.image`Dart1`, mySprite, 0, -150)\n    projectile.startEffect(effects.ashes)\n})\nsprites.onOverlap(SpriteKind.Player, SpriteKind.Gas, function (sprite, otherSprite) {\n    statusbar.value = 100\n    otherSprite.destroy()\n})\nstatusbars.onZero(StatusBarKind.Energy, function (status) {\n    game.over(false)\n})\nsprites.onOverlap(SpriteKind.Projectile, SpriteKind.Enemy, function (sprite, otherSprite) {\n    sprite.destroy(effects.fire, 100)\n    otherSprite.destroy()\n    info.changeScoreBy(1)\n})\nsprites.onOverlap(SpriteKind.Player, SpriteKind.Enemy, function (sprite, otherSprite) {\n    info.changeLifeBy(-1)\n    otherSprite.destroy(effects.disintegrate, 500)\n})\nlet myEnemy: Sprite = null\nlet myFuel: Sprite = null\nlet projectile: Sprite = null\nlet statusbar: StatusBarSprite = null\nlet mySprite: Sprite = null\nscene.setBackgroundImage(assets.image`Galaxy`)\nmySprite = sprites.create(assets.image`Rocket`, SpriteKind.Player)\ncontroller.moveSprite(mySprite, 100, 100)\nmySprite.setFlag(SpriteFlag.StayInScreen, true)\nstatusbar = statusbars.create(20, 4, StatusBarKind.Energy)\nstatusbar.attachToSprite(mySprite, -30, 0)\nmySprite.y = 100\ngame.onUpdateInterval(5000, function () {\n    myFuel = sprites.createProjectileFromSide(assets.image`Fuel`, 0, 100)\n    myFuel.x = randint(5, 155)\n    myFuel.setKind(SpriteKind.Gas)\n})\ngame.onUpdateInterval(2000, function () {\n    myEnemy = sprites.createProjectileFromSide(assets.image`Spider`, 0, 50)\n    myEnemy.x = randint(5, 155)\n    myEnemy.setKind(SpriteKind.Enemy)\n})\ngame.onUpdateInterval(300, function () {\n    statusbar.value += -1\n})\n",
  "pxt.json": "{\n    \"name\": \"Fuel Up\",\n    \"description\": \"\",\n    \"dependencies\": {\n        \"device\": \"*\",\n        \"pxt-status-bar\": \"github:jwunderl/pxt-status-bar\"\n    },\n    \"files\": [\n        \"main.blocks\",\n        \"main.ts\",\n        \"README.md\",\n        \"assets.json\",\n        \"images.g.jres\",\n        \"images.g.ts\",\n        \"tilemap.g.jres\",\n        \"tilemap.g.ts\"\n    ],\n    \"targetVersions\": {\n        \"branch\": \"v1.4.43\",\n        \"tag\": \"v1.4.43\",\n        \"commits\": \"https://github.com/microsoft/pxt-arcade/commits/d613fa29c3c700e4b7d90381ed39b7e65b0de04e\",\n        \"target\": \"1.4.43\",\n        \"pxt\": \"6.12.25\"\n    },\n    \"preferredEditor\": \"blocksprj\"\n}\n",
  "tilemap.g.jres": "{\n    \"transparency16\": {\n        \"data\": \"hwQQABAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA==\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"tilemapTile\": true\n    },\n    \"*\": {\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"dataEncoding\": \"base64\",\n        \"namespace\": \"myTiles\"\n    }\n}",
  "tilemap.g.ts": "// Auto-generated code. Do not edit.\nnamespace myTiles {\n    //% fixedInstance jres blockIdentity=images._tile\n    export const transparency16 = image.ofBuffer(hex``);\n\n    helpers._registerFactory(\"tile\", function(name: string) {\n        switch(helpers.stringTrim(name)) {\n            case \"transparency16\":return transparency16;\n        }\n        return null;\n    })\n\n}\n// Auto-generated code. Do not edit.\n"
}
```