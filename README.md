# Calling Multiple LLMs with Spring AI

This project demonstrates how to configure and call two different Large Language Models (LLMs) in the same Java application using Spring AI. Specifically, it shows how to integrate OpenAI and Anthropic models within a Spring Boot application.

## Project Overview

This Spring Boot application showcases the following features:

- Integration with Spring AI
- Configuration of both OpenAI and Anthropic chat models
- Separate REST endpoints for each LLM
- Usage of ChatClient for interacting with the models

## Prerequisites

Before you begin, ensure you have the following installed:

- Java Development Kit (JDK) 23 or later
- Apache Maven
- An OpenAI API key
- An Anthropic API key

## Project Structure

The project consists of the following key components:

- `Application.java`: The main Spring Boot application class
- `OpenAiChatController.java`: Controller for OpenAI interactions
- `AnthropicChatController.java`: Controller for Anthropic interactions
- `pom.xml`: Maven configuration file

## Getting Started

1. Clone the repository:

```bash
git clone https://github.com/yourusername/multiple-llms.git
cd multiple-llms
```

2. You can disable the ChatClient.Builder autoconfiguration by setting the property:

```bash
spring.ai.chat.client.enabled=false
```

3. Set up your API keys:

Create an `application.properties` file in the `src/main/resources` directory and add your API keys:

```properties
spring.ai.openai.api-key=your_openai_api_key
spring.ai.anthropic.api-key=your_anthropic_api_key
```

4. Build the project:

```bash
mvn clean install
```

5. Run the application:

```bash
mvn spring-boot:run
```

## Usage

Once the application is running, you can interact with the LLMs using the following endpoints:

- OpenAI: `http://localhost:8080/openai`
- Anthropic: `http://localhost:8080/claude`

Each endpoint will return an interesting fact about the respective company.

## Code Explanation

### Application Configuration

In `Application.java`, we define two `ChatClient` beans, one for each LLM:

```java
@Bean
public ChatClient openAIChatClient(OpenAiChatModel chatModel) {
    return ChatClient.create(chatModel);
}

@Bean
public ChatClient anthropicChatClient(AnthropicChatModel chatModel) {
    return ChatClient.create(chatModel);
}
```

### Controller Implementation

Each controller is injected with the appropriate `ChatClient` using the `@Qualifier` annotation:

```java
@RestController
public class OpenAiChatController {

    private final ChatClient chatClient;

    public OpenAiChatController(@Qualifier("openAIChatClient") ChatClient chatClient) {
        this.chatClient = chatClient;
    }

    @GetMapping("/openai")
    public String home() {
        return chatClient.prompt()
                .user("Tell me an interesting fact about OpenAI")
                .call()
                .content();
    }
}
```

The Anthropic controller follows a similar pattern.

## Dependencies

This project uses the following key dependencies:

- Spring Boot Starter Web
- Spring AI OpenAI Spring Boot Starter
- Spring AI Anthropic Spring Boot Starter

The complete list of dependencies can be found in the `pom.xml` file.

## Customization

To customize the prompts or add more functionality:

1. Modify the controller methods to accept user input.
2. Adjust the prompts in the `chatClient.user()` calls.
3. Add new endpoints for different types of interactions with the LLMs.

## Troubleshooting

If you encounter any issues:

1. Ensure your API keys are correctly set in the `application.properties` file.
2. Check that you're using a compatible Java version (23 or later).
3. Verify that all dependencies are correctly resolved by Maven.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is open source and available under the [MIT License](LICENSE).

---

Happy coding with Spring AI and multiple LLMs!