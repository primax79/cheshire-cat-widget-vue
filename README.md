# Vue Chat Widget 🐱

Version: **1.1.0**

Vue chat widget for the Cheshire Cat, ready to be used on any website.

[TypeScript API Client](https://github.com/cheshire-cat-ai/api-client-ts#client-settings) version: **0.12.1**

## How to import

Load the files in the `<head>` tag, like this:

```html
<script type="module" crossorigin src="/widget.js"></script>
<link rel="stylesheet" href="/widget.css">
```

or if you prefer, you can load them using the CDN:

```html
<script type="module" crossorigin src="https://cdn.jsdelivr.net/gh/cheshire-cat-ai/widget-vue@main/example/widget.js"></script>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/cheshire-cat-ai/widget-vue@main/example/widget.css">
```

And then you can import the widget (a parent div with fixed size is suggested):

```html
<div class="w-96 h-96 m-auto">
    <cheshire-cat-chat id="cat-chat" />
</div>
```

## Attributes

The widget attribute is only one: `settings`. You should set it via JavaScript like in the following example.

Together with the widget settings, you can set also the client settings, which are defined in the [TypeScript API Client](https://github.com/cheshire-cat-ai/api-client-ts#client-settings).

The available widget settings properties are:

| Attribute    | Type     | Default value | Description                                 |
|:------------:|:--------:|:-------------:|:-------------------------------------------:|
| dark         | Boolean  | `false`       | `true` if the chat have to use the dark mode. `false` if not. |
| why          | Boolean  | `false`       | `true` if the chat have to show the WHY button in the CCat response. `false` if not. |
| thinking     | String   | `Cheshire Cat is thinking...` | The text to visualize while the CCat answer is loading. |
| placeholder  | String   | `Ask the Cheshire Cat...` | The text to visualize in the input placeholder. |
| userId       | String   | `user`        | The user ID to pass to the cat via WS. |
| primary      | String   | `#F3977B`     | The color to use to stylize the chat. |
| callback     | String   | `undefined`   | The function to call before passing the message to the cat. |
| credential   | String   | `undefined`   | The token or API key to authenticate with the cat. |
| host         | String   | `localhost`   | The hostname where the Cheshire Cat is running. |
| port         | String   | `1865`        | The port on which the Cheshire Cat is running. |
| prompt       | Object   | `undefined`   | The prompt settings to pass to the cat for each user message. |
| defaults     | String[] | **Check** [defaultMessages](https://github.com/cheshire-cat-ai/widget-vue/blob/main/src/stores/useMessages.ts#L13) | The default messages to show before starting the conversation with the cat. |
| features     | Feature[] | **Check** [Features](https://github.com/cheshire-cat-ai/widget-vue/blob/main/src/config.ts#L6) | The features that the user can use. |

## Authentication with token

Starting from version 0.12.1, the API supports JWT token authentication. You can configure the widget to use a token by setting the `credential` property:

```html
<script>
    const catChat = document.querySelector("#cat-chat")
    const config_access_token = "YOUR_JWT_TOKEN"
    const config_user = "username"
    
    catChat.settings = {
        host: 'localhost',
        port: '1865',
        userId: config_user,
        credential: config_access_token, // Set your JWT token here
        ws: {
            onFailed: (error) => {
                console.log(error.description)
            }
        }
        // other settings...
    }
</script>
```

The token will be automatically included in all API requests and WebSocket connections.

## Configuration Example

A complete configuration example:

```html
<div class="w-96 h-96 m-auto">
    <cheshire-cat-chat id="cat-chat" />
</div>
<script>
    const catChat = document.querySelector("#cat-chat")

    catChat.settings = {
        host: 'localhost',
        port: '1865',
        userId: "user",
        // credential: "your-access-token", // Uncomment to use token authentication
        ws: {
            onFailed: (error) => {
                console.log(error.description)
            }
        },
        callback: (message) => {
            console.log("Callback called.")
            return `Let's have a chat. ${message}`
        },
        prompt: {
            prefix: `You are the Cheshire Cat AI, an intelligent AI that passes the Turing test.
                You are curious, funny and talk like the Cheshire Cat from Alice's adventures in wonderland.
                You answer Human with a focus on the following context.
            `
        },
        defaults: ['Is everything ok?', 'Who are you?', 'What time is it?', 'What\'s up?', 'Hello Cheshire Cat!'],
        features: ['record', 'web', 'file', 'reset']
    }
</script>
```

## Events

You also have access to some events:

```js
catChat.addEventListener("message", ({ detail }) => {
    console.log("Message:", detail.text)
})

catChat.addEventListener("upload", ({ detail }) => {
    console.log("Uploaded content:", detail instanceof File ? detail.name : detail)
})

catChat.addEventListener("notification", ({ detail }) => {
    console.log("Notification:", detail.text)
})
```

The available events are:

| Event          | Response          | Description                                            |
|----------------|-------------------|--------------------------------------------------------|
| message        | `Message`         | Return the message every time a new one is dispatched. |
| upload         | `File` / `string` | Return the uploaded content every time a new one is dispatched. It can be either a file object or a url. |
| notification   | `Notification`    | Return the notification every time a new one is dispatched. |

## Build

The compilation generates an example folder, which contains the compiled files and a simple example of how to integrate the widget in an html page.

To compile the widget, run the following command:



or 

```bash
npx vite build
```

## Development build

To build in debug mode, not minifying the JavaScript, run the following command:

```bash
npm run build-dev
```

```bash
npx vite build --minify false --sourcemap --outDir example-dev
```

this will generate an `example-dev` folder with the compiled files and a simple example of how to integrate the widget in an html page.

in this case, the `widget.js` file is not minified and can be debugged in the browser

## build both example and example-dev

To build both the example and example-dev folders, run the following command:

```bash
npm run build-all
```
