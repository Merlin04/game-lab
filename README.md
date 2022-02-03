# 👾 **[Hack Club Game Lab →](https://game-lab.hackclub.dev/)**

The best way to learn is by making things you care about and sharing them with others. **That's what Game Lab is all about**.

Have you ever wanted to...

- Make Pong in 30 lines of code?
- Create the Chrome dino game in 50?
- Or... even better... make a delightful game that doesn't exist yet?

Then get building with Game Lab!

You should be able to get started in Game Lab with very little experience programming but you should still be able to have fun with it even if you're an expert. Enjoy and we'd love to see what you make!

### ⮑ _**[Click here to launch Game Lab](https://game-lab.hackclub.dev/)**_ ⮐
_(and scroll down for a brief tutorial to get started)_
!

## Tutorial / How To Get Started Building Games

Learning from functional examples is the best way to get started building games with Game Lab.

Below we have created a series of short examples with code that works, that grow from creating a character on the screen to creating a more complicated game.

Initialize the game engine (this should already be written for you when you open the editor)

```js
const e = createEngine(gameCanvas, 300, 300);
```

---

Make a character:

<img width="345" alt="Screen Shot 2022-01-13 at 10 50 41 AM" src="https://user-images.githubusercontent.com/27078897/149362983-6f82a61c-c3d5-40b7-920f-f673c3ff2646.png">

```js
const e = createEngine(gameCanvas, 300, 300);

e.add({
  x: 188,
  y: 69,
  sprite: test_sprite,
  scale: 4, // this makes the sprite larger than its default 32 x 32 pixel size
})

e.start();
```

---

Add a floor with gravity:

<img width="345" alt="Screen Shot 2022-01-13 at 10 50 41 AM" src="https://user-images.githubusercontent.com/27078897/149363706-453a45e8-d0d4-44a3-acc3-09e4bb577392.gif">

```js
const e = createEngine(gameCanvas, 300, 300);

e.add({
  solid: true, // the solid property makes this object collideable
  x: 135,
  y: 114,
  sprite: test_sprite,
  scale: 4,
  update(me) { // update runs every frame
    me.vy += 2; // adding velocity every frame is acceleration
  }
})

e.add({
  solid: true, // the solid property makes this object collideable
  x: -6,
  y: 283,
  sprite: floor,
  scale: 11,
})

e.start();
```

---

Add movement:

<img width="345" alt="Screen Shot 2022-01-13 at 10 50 41 AM" src="https://user-images.githubusercontent.com/27078897/149365452-7b042996-2beb-40a8-866e-f5748b5631da.gif">

```js
const e = createEngine(gameCanvas, 300, 300);

e.add({
  solid: true,
  x: 135,
  y: 114,
  sprite: test_sprite,
  scale: 4,
  update(me) {
    me.vy += 2;

    // we can add key inputs by checking the keys in the update loop
    if (e.heldKey("ArrowLeft")) me.x -= 3;
    if (e.heldKey("ArrowRight")) me.x += 3; 
  }
})

e.add({
  solid: true,
  x: -6,
  y: 283,
  sprite: floor,
  scale: 11,
})

e.start();
```

---

Add jump:

<img width="345" alt="Screen Shot 2022-01-13 at 10 50 41 AM" src="https://user-images.githubusercontent.com/27078897/149366181-588ae196-03dd-4268-9907-9477caa8a834.gif">

```js
const e = createEngine(gameCanvas, 300, 300);

e.add({
  tags: ["player"], // we can add tags so we can reference objects
  solid: true,
  x: 135,
  y: 114,
  sprite: test_sprite,
  scale: 4,
  collides(me, them) { // this runs when we collide with another object
    if (e.pressedKey(" ")) {
      // here we are checking if we are standing on the floor
      if (them.hasTag("floor")) me.vy -= 19;
    }
  },
  update(me) {
    me.vy += 2;

    if (e.heldKey("ArrowLeft")) me.x -= 3;
    if (e.heldKey("ArrowRight")) me.x += 3;
  }
})

e.add({
  tags: ["floor"], // we can add tags so we can reference objects
  solid: true,
  x: -6,
  y: 283,
  sprite: floor,
  scale: 11,
})

e.start();
```

---

Add platforms:

<img width="345" alt="Screen Shot 2022-01-13 at 10 50 41 AM" src="https://user-images.githubusercontent.com/27078897/149367516-7edb2780-edbd-4977-9a07-dcfbf47fcf93.gif">

```js
const e = createEngine(gameCanvas, 300, 300);
const ctx = e.ctx;

e.add({
  tags: ["player"],
  sprite: test_sprite,
  scale: 3,
  solid: true,
  x: 50,
  y: 16,
  collides(me, them) {
    if (them.hasTag("platform")) me.vx = them.vx;
    
    if (e.pressedKey(" ")) {
      if (them.hasTag("platform")) me.vy -= 11;
    }
  },
  update: (me) => {
      me.vy += 0.4;
  
      if (e.heldKey("ArrowLeft")) me.x -= 3;
      if (e.heldKey("ArrowRight")) me.x += 3;
  },
})

// we can use a function to make multiple instances of an object
const addPlatform = (x, y) => e.add({
  tags: ["platform"],
  sprite: floor,
  scale: 3,
  solid: true,
  x: x,
  y: y,
  vx: -1,
  bounce: -1, // bounce determines how much velocity changes when we collide with something
  update: (me) => {
      if (me.x < 0) me.vx = 1;
      if (me.x + me.width > e.width) me.vx = -1
  },
})

addPlatform(50, 200);
addPlatform(20, 100);

e.start();
```

---

Collections:

<img width="333" alt="Screen Shot 2022-01-13 at 11 21 43 AM" src="https://user-images.githubusercontent.com/27078897/149369879-7d384b3a-2f15-4816-a59e-76b56bb9a944.gif">

```js
const e = createEngine(gameCanvas, 300, 300);
const ctx = e.ctx;

e.add({
  tags: ["player"],
  sprite: test_sprite,
  scale: 2,
  x: 150,
  y: 50,
  update: (me) => {
    if (e.heldKey("ArrowUp")) me.y -= 3;
    if (e.heldKey("ArrowDown")) me.y += 3;
  },
})

e.add({
  tags: ["target"],
  sprite: floor,
  scale: 3,
  x: 112,
  y: 232,
  collides(me, them) {
    if (them.hasTag("player")) {
      e.remove("target"); // we can remove objects by their tag name
    }
  }
})

e.start();
```

---

Add a background:

<img width="333" alt="Screen Shot 2022-01-13 at 11 21 43 AM" src="https://user-images.githubusercontent.com/27078897/149368356-c343a214-0d31-4d5f-a2d4-d0575b18047b.png">

```js
const e = createEngine(gameCanvas, 300, 300);

e.add({
  update() { // we can also draw on the game canvas
    e.ctx.fillStyle = "pink";
    e.ctx.fillRect(0, 0, e.width, e.height);
  }
})

e.start();
```

---

Refer to the following example for all the available object properties:

```js
e.add({
  tags: ["tag-name"], // assign tags to later reference object
  solid: true, // add solid property to make object bump into other solids
  x: 178, // x position
  y: 126, // y position
  vx: 1, // x velocity
  vy: 3, // y velocity
  sprite: ball,
  scale: 2,
  rotate: 90, // rotate by some degrees
  bounce: 1, // how much velocity is lost on collisions
  origin: [0, 0], // this moves the origin of the object
  collides(me, them) { // function run on collision
    if (them.hasTag("tag-name")) {} // check tag names to figure out what you've collided with
  },
  update: (me) => {
    if (e.heldKey("ArrowDown")) me.y += 3; // add key inputs
    if (e.pressedKey("ArrowUp")) me.y -= 3; // add key inputs

    if (me.x < 0) {
      e.end(); // end the game
      e.addText("Game Over", e.width/2, e.height/2, { color: "blue", size: 32 }) // add text
    }
  },
})
```

## Tiny Games

[Pong-ish](https://game-lab.hackclub.dev/?id=bd5087c16160988e4d2e27ca2c6157f9)

<img width="345" alt="Screen Shot 2022-01-13 at 10 50 41 AM" src="https://user-images.githubusercontent.com/27078897/149371012-faf3e45f-9d3a-47d4-831b-566d9171d2bd.gif">

---

[Crappy Birds](https://game-lab.hackclub.dev/?id=c66c509f01eb83f7bc67053818b75159)

<img width="345" alt="Screen Shot 2022-01-13 at 10 50 41 AM" src="https://user-images.githubusercontent.com/27078897/149380918-a1855ab3-cc2d-4a9a-adc0-d5316d6f17ba.gif">

---

[Brick Broken](https://game-lab.hackclub.dev/?id=8f18ab06c1a8f5267e6f4a6b3bec06f5)

<img width="345" alt="Screen Shot 2022-01-13 at 10 50 41 AM" src="https://user-images.githubusercontent.com/27078897/150606449-5b73d7fe-f2d3-432f-9cc5-346c20919ec8.gif">

## My game used to work but now it doesn't?

This could be because you made your game in an old version of Game Lab.

Game Lab is in active development. We want to make sure you can play the games you make even if they aren't compatible with the newest version of the editor. If you made a game and need to run it on an old version of Game Lab you can use this site: 

`https://game-lab-versions.hackclub.dev/[SEMANTIC_VERSION]/index.html`

For example the first release of Game Lab is available here: 

https://game-lab-versions.hackclub.dev/0.1.0/index.html


## Philosophy

As we said before people learn best when they make things that they care about which they can share with others. This learning philosophy is called [constructionism](https://en.wikipedia.org/wiki/Constructionism_(learning_theory)) and Game Lab is a type of microworld. It's an environment where you can discover programming by using it to express yourself. 

Game Lab could also be considered a minimalist [fantasy console](https://en.wikipedia.org/wiki/Fantasy_video_game_console#:~:text=A%20fantasy%20video%20game%20console,their%20fictional%20hardware%20will%20have.) sort of like [Pico-8](https://www.lexaloffle.com/pico-8.php).

## Development

Join `#gamelab-dev` on the [Hack Club Slack](https://hackclub.com/slack/) to join the development discussion

The Hack Club Game Lab requires a local HTTP server to run in development. Here's how to get it running on your own machine.

Clone repo:

    $ git clone https://github.com/hackclub/game-lab

Start a local HTTP server inside the repo:

    $ cd game-lab/
    $ python3 -m http.server 3000

And then go to http://localhost:3000 in your web browser, and it should work!

## License

The Hack Club Game Lab is open source and licensed under the [MIT License](./LICENSE). Fork, remix, and make it your own! Pull requests and other contributions greatly appreciated.
