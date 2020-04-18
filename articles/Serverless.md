"serverless"不是没有server, 开发者只关心应用功能的实现, 而不是怎么搭建础设施

## How Serverless Works

 Functions as a Service (FaaS) 

1. **The developer writes a function.** This function often serves a specific purpose within the application code.
2. **The developer defines an event.** The event is what triggers the cloud service provider to execute the function. A common type of event is an HTTP request.
3. **The event is triggered.** If the defined event is an HTTP request, a user triggers the event with a click or similar action.
4. **The function is executed.** The cloud service provider checks if an instance of the function is already running. If not, it starts a new one for the function.
5. **The result is sent to the client.** The user sees the result of the executed function inside the application.