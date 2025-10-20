Bugs: 
- lines are drawn off center (slightly low)
- there's a noticeable lag between moving the pointer and seeing the line
- the lines blink a lot as I'm moving the mouse

Make sure there's no blinking. I'd suggest re-drawing only when necessary or use something like double buffering to make sure the user doesn't see incomplete images
