## SceneMan

Scene Man is a simple and lightweight scene/gamestate manager! (No relation to [lucassardois/sceneman](https://github.com/lucassardois/sceneman))

It is useful for separating your game into distinct “screens” that are stored in separate files. It can also be used to easily separate game logic from GUI logic.

### Features:

*   Stack-based: Multiple scenes can be layered over one another at the same time. Scenes can also push and pop other scenes from the stack at any time.
*   Extremely flexible: custom callbacks are trivial to define and can be used in any situation (updating, drawing, detecting mouse clicks, etc).
*   Small size: The entire system is only a few hundred lines long. It should take up next to no space inside your projects.
*   Portable: Works in any Lua-based frameworks or game engines. Tested with Lua version 5.1.
*   Stack freezing: The scene stack can be “frozen” so the scenes only transition when you want them to.
*   Stack saving: The current contents of the stack can be saved and restored with ease.

### Usage:

See the [Example](https://github.com/KINGTUT10101/SceneMan/wiki/Example) page on the wiki for a general sample.

See the [Freezing](https://github.com/KINGTUT10101/SceneMan/wiki/Freezing) page on the wiki for an example of the freezing system.

![image](https://github.com/KINGTUT10101/SceneMan/assets/45105509/4df08b3f-3235-4a5d-91ca-5073b5924a50)

### Documentation:

#### Attributes:

```plaintext
sceneMan.scenes = {} -- All created scenes will be stored here.
sceneMan.stack = {} -- Scenes that are pushed will be stored here.
sceneMan.shared = {} -- Stores variables that are shared between scenes
sceneMan.saved = {} -- Stores saved stacks so they can be restored later
sceneMan.buffer = {} -- Stores the scene stack when the original scene stack is disabled
sceneMan.frozen = false -- If true, the buffer will be used instead of the original stack
sceneMan.version = "1.2" -- The used version of Scene Man
```

#### Methods:

```plaintext
--- Adds a new scene to Scene Man and initializes it via its load method.
-- This will call the scene's "load" method
-- @param name (string) The name of the new scene. This will be used later to push, insert, and remove this scene from the stack
-- @param scene (table) A table containing the scene's attributes and callback functions
-- @param ... (varargs) A series of values that will be passed to the scene's "load" callback
sceneMan:newScene (name, scene, ...)

--- Removes a scene from Scene Man and calls its delete method.
-- This will call the scene's "delete" method
-- If you try to push or insert a deleted scene, Scene Man will throw an error!
-- @param name (string) The name of the scene that should be deleted
-- @param ... (varargs) A series of values that will be passed to the scene's "delete" callback
sceneMan:deleteScene (name)

--- Returns the current size of the stack.
-- @return (int) The size of the stack
sceneMan:getStackSize ()

--- Gives the name of the current scene. It will return nil is there are no scenes on the stack.
-- @return (string) The name of the scene at the top of the stack
sceneMan:getCurrentScene ()

--- Adds a scene from the scenes table onto the stack.
-- This will call the scene's "whenAdded" method
-- Scenes at the top of the stack will have their functions called last
-- @raise When the given scene name isn't registered inside Scene Man
-- @param name (string) The name of the scene to add to the top of the stack
-- @param ... (varargs) A list of values that will be passed to the event's "whenAdded" callback function
sceneMan:push (name, ...)

--- Pops a scene off of the stack.
-- This will call the topmost scene's "whenRemoved" method
-- @param ... (varargs) A list of values that will be passed to the event's "whenRemoved" callback function
sceneMan:pop (...)

--- Adds a scene to the stack at a given index.
-- This will call the scene's "whenAdded" method
-- @raise When the given scene name isn't registered inside Scene Man
-- @param name (string) The name of the scene to add to the top of the stack
-- @param index (int) The position within the stack that the scene should be inserted at
-- @param ... (varargs) A list of values that will be passed to the event's "whenAdded" callback function
-- @return (bool) True if the operation was successful
sceneMan:insert (name, index, ...)

--- Removes a scene from the stack at a certain index.
-- This will call the scene's "whenRemoved" method
-- @param index (int) The position within the stack that the scene should be removed at
-- @param ... (varargs) A list of values that will be passed to the event's "whenRemoved" callback function
-- @return (bool) True if the operation was successful
sceneMan:remove (index, ...)

--- Removes all scenes from the stack.
-- This will call all the scenes' "whenRemoved" methods, starting from the topmost scene
-- @param ... (varargs) A list of values that will be passed to the event's "whenRemoved" callback function
sceneMan:clearStack (...)

--- Fires an event callback for all scenes on the stack.
-- @param eventName (string) The name of the event
-- @param ... (varargs) A series of values that will be passed to the scenes' event callbacks
sceneMan:event (eventName, ...)

--- Redirects stack-altering operations into the buffer instead.
sceneMan:freeze ()

--- Copies the changes from the buffer back into the original stack.
sceneMan:unfreeze ()

--- Saves the current state of the stack so it can be restored later.
-- This will save the frozen buffer if the stack is frozen
-- This will not modify the current stack in any way
-- @param id (string) A unique ID that will be used to identify the saved stack. It will override anything currently stored at that ID
sceneMan:saveStack (id)

--- Loads a stack from the saved table.
-- This will call the loaded scenes' "whenAdded" methods
-- @param id (string) A unique ID that identifies the stack that should be restored
-- @param ... (varargs) A list of values that will be passed to the event's "whenAdded" callback function
-- @return True if the stored stack at the given ID exists and if the current stack is empty, otherwise false
sceneMan:restoreStack (id, ...)

--- Removes a saved stack permanently.
-- This will not delete the scenes in the stack
-- This will not affect the current stack, even if it was restored using the to-be-deleted stack
-- @param id (string) A unique ID that identifies the stack that should be deleted
sceneMan:deleteStack (id)
```