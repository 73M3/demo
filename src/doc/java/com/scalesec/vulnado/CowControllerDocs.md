# CowController.java: Cow Say Controller

## Overview
This controller handles HTTP requests to the `/cowsay` endpoint, generating a response using the Cowsay utility.

## Process Flow
```mermaid
flowchart TD
    A["HTTP Request to /cowsay"] --> B["Invoke cowsay method"]
    B --> C["Call Cowsay.run(input)"]
    C --> D["Return Cowsay output"]
```

## Insights
- The controller is designed to handle HTTP GET requests to the `/cowsay` endpoint.
- It uses the `Cowsay` utility to generate a response based on the provided input.
- The default input message is "I love Linux!" if no input is provided.

## Dependencies
```mermaid
flowchart LR
    CowController --- |"Calls"| Cowsay
```
- `Cowsay`: Description of parameters provided when calling it and the nature of the relation and eventual longer description of it
