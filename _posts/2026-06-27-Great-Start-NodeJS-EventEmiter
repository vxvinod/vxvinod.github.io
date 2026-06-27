---
title: "Understanding Node.js EventEmitter: From a Todo App to cnMaestro"
published: true
tags: nodejs, javascript, backend, networking
---

# Understanding Node.js EventEmitter: From a Todo App to cnMaestro

If you've used Node.js for more than five minutes, you've used `EventEmitter` even if you didn't know it — it's the engine under streams, `http.Server`, `process`, and basically every "something happened, react to it" mechanism in Node core.

This post walks through the concept with two examples: a small Todo REST API (the simplest possible case), and a mapping to **cnMaestro**, Cambium Networks' wireless management platform — because once you see the same shape in a real network management system, the pattern clicks.

## The Core Idea: Publish / Subscribe

`EventEmitter` (from Node's built-in `events` module) lets one piece of code **announce** that something happened, without knowing or caring who's listening:

```js
const EventEmitter = require('events');
const emitter = new EventEmitter();

emitter.on('greet', (name) => console.log(`Hello, ${name}!`));
emitter.emit('greet', 'World'); // -> "Hello, World!"
```

`.on()` registers a listener for a named event. `.emit()` fires that event, synchronously calling every listener registered for it, in registration order, passing along whatever arguments you give it.

```mermaid
flowchart LR
    E[".emit('eventName', data)"] --> Bus(("EventEmitter"))
    Bus --> L1["Listener 1: .on('eventName', ...)"]
    Bus --> L2["Listener 2: .on('eventName', ...)"]
    Bus --> L3["Listener 3: .on('eventName', ...)"]
```

The emitting code never references `L1`, `L2`, or `L3` directly. You can add or remove listeners independently without touching the code that calls `.emit()`. That decoupling is the entire point.

## Example 1: A Todo App Built on Events

Here's a small Express Todo API where each CRUD route announces what it did, instead of hardcoding side effects like stats-tracking and logging directly into the route handler:

```js
const express = require('express');
const app = express();
const PORT = 3000;
const fs = require('fs').promises;
const DATA_FILE = './todo.json';
const EventEmitter = require('events');
const todoEvents = new EventEmitter();

app.use(express.json());

let todos = [];
let id = 1;
let stats = { created: 0, updated: 0, deleted: 0 };

async function loadTodos() {
    try {
        const data = await fs.readFile(DATA_FILE, 'UTF-8');
        return JSON.parse(data);
    } catch (err) {
        return [];
    }
}

async function saveTodos(todos) {
    return fs.writeFile(DATA_FILE, JSON.stringify(todos, null, 2));
}

app.post('/todos', async (req, res) => {
    const todo = { id: id++, task: req.body.task, done: false };
    todos.push(todo);
    await saveTodos(todos);
    todoEvents.emit('todo:created', todo);
    res.status(201).json(todo);
});

app.put('/todo/:id', async (req, res) => {
    let todo = todos.find(t => t.id === parseInt(req.params.id));
    if (!todo) return res.status(404).json({ message: 'todo not found' });

    todo.task = req.body.task !== undefined ? req.body.task : todo.task;
    todo.done = req.body.done !== undefined ? req.body.done : todo.done;
    await saveTodos(todos);
    todoEvents.emit('todo:updated', todo);
    res.json(todo);
});

app.delete('/todo/:id', async (req, res) => {
    todos = todos.filter(t => t.id !== parseInt(req.params.id));
    await saveTodos(todos);
    todoEvents.emit('todo:deleted', req.params.id);
    res.json({ message: 'deleted' });
});

async function logEvent(message) {
    const timestamp = new Date().toISOString();
    await fs.appendFile('./app.log', `[${timestamp}] ${message}\n`);
}

todoEvents.on('todo:created', (todo) => {
    stats.created++;
    logEvent(`Todo Created - ${todo.id} - ${todo.task}`);
});

todoEvents.on('todo:updated', (todo) => {
    stats.updated++;
    logEvent(`Todo Updated - ${todo.id} - ${todo.task} - ${todo.done}`);
});

todoEvents.on('todo:deleted', (todoId) => {
    stats.deleted++;
    logEvent(`Todo Deleted - ${todoId}`);
});

// Without this listener, an emitted 'error' event would crash the process
todoEvents.on('error', (err) => {
    logEvent(`Error message ${err.message}`);
});

app.get('/stats', (req, res) => res.json(stats));
```

Notice the route handlers don't know `stats` or `app.log` exist. `app.post('/todos', ...)` just does its job and fires `todoEvents.emit('todo:created', todo)` — two completely separate listeners react independently:

```mermaid
sequenceDiagram
    participant Client
    participant Route as POST /todos
    participant Bus as todoEvents (EventEmitter)
    participant Stats as stats listener
    participant Log as logEvent listener

    Client->>Route: POST /todos { task }
    Route->>Route: push to todos[], saveTodos()
    Route->>Bus: emit('todo:created', todo)
    Bus->>Stats: stats.created++
    Bus->>Log: append line to app.log
    Route->>Client: 201 Created
```

Want a fourth behavior — say, a webhook call whenever a todo is created? Add one more `todoEvents.on('todo:created', ...)` listener. Nothing else in the file changes.

## Example 2: The Same Pattern at Network Scale — cnMaestro

cnMaestro is Cambium Networks' management platform for wireless infrastructure — it watches access points (APs) and subscriber modules (SMs) and reacts to state changes: a device going online/offline, an RF threshold being breached, a config push completing.

Architecturally, it's solving the exact same problem as the Todo app: **one state change, multiple independent reactions** — dashboard counters update, an alarm gets logged, NOC gets notified — none of which should be hardcoded together.

| cnMaestro concept | EventEmitter concept |
|---|---|
| AP/SM reports a state change (online, offline, RF alarm) | `.emit('event-name', data)` |
| cnMaestro's Alarm Manager reacting to that change | `.on('event-name', callback)` |
| Dashboard counters, alerts, and event logs all reacting to the same device event | Multiple independent `.on()` listeners for one `.emit()` |
| The AP doesn't know who's watching it | The emitter doesn't know/care who's listening |

The same code shape, cnMaestro-flavored:

```js
const EventEmitter = require('events');
const deviceEvents = new EventEmitter();

let stats = { online: 0, offline: 0, alarms: 0 };

// Listener 1 — dashboard counters
deviceEvents.on('device:online', () => stats.online++);
deviceEvents.on('device:offline', () => stats.offline++);
deviceEvents.on('device:alarm', () => stats.alarms++);

// Listener 2 — alarm log
deviceEvents.on('device:alarm', (d) => {
    console.log(`[ALARM] ${d.id} ${d.metric}=${d.value}`);
});

// Listener 3 — NOC notification, completely independent of the above
deviceEvents.on('device:offline', (d) => {
    console.log(`Notify NOC: ${d.id} at ${d.site} went down`);
});

// Somewhere, a poller/webhook handler detects real device state and announces it:
deviceEvents.emit('device:offline', { id: 'AP-101', site: 'Rooftop-A' });
deviceEvents.emit('device:alarm', { id: 'AP-101', metric: 'RF_SNR', value: 4 });
```

```mermaid
flowchart LR
    AP["AP / SM device state change"] -- detected by poller --> DE(("deviceEvents.emit(...)"))
    DE --> Dash["Dashboard counters"]
    DE --> Alarm["Alarm log"]
    DE --> NOC["NOC notification"]
```

Same diagram shape as the Todo app's sequence diagram — just swap "todo created" for "device offline."

## Gotchas Worth Knowing

- **Listeners run synchronously**, in registration order, on the same call stack as `.emit()`. It's not deferred like a microtask — `.emit()` doesn't return until every listener has run (though if a listener itself kicks off an async operation, like `logEvent`'s `fs.appendFile`, that part still resolves later).
- **`'error'` is a special-cased event name.** If you `.emit('error', ...)` and there's no listener registered for it, Node throws and crashes the process. Always register an `'error'` listener on any `EventEmitter` you create, like the Todo app does.
- **Default max listeners is 10** per event name — `EventEmitter` will print a warning past that, usually a sign of a listener leak. Adjustable via `setMaxListeners()`.
- `EventEmitter` isn't just a utility class — it's the base behavior for `stream.Readable/Writable`, `http.Server`, `child_process`, and `process` itself. Understanding it is understanding a big chunk of how Node's core APIs are shaped.

## Takeaway

Whether it's a todo getting created or a wireless AP dropping off the network, the underlying shape is identical: **decouple the thing that detects a change from the things that react to it.** `EventEmitter` is Node's built-in tool for exactly that, and once you see it in a toy Todo app, you start recognizing it everywhere — including in production network management platforms like cnMaestro.
