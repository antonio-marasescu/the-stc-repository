# Leveraging LLMs in Software Development

This document offers some practical use cases for integrating Large Language Models (LLMs) into your development workflow.
It is of course, opinionated and based on personal observation.

## Table of Contents

1. [Leveraging LLMs in Software Development](#leveraging-llms-in-software-development)
2. [Recommended Tools](#recommended-tools)
3. [Best Practices](#best-practices)
4. [LLM Use Cases in Development](#llm-use-cases-in-development)
   - [Feature Development](#feature-development)
     - [Large-Scale Features](#large-scale-features)
     - [Small-Scale Features](#small-scale-features)
   - [Exploratory Use Cases](#exploratory-use-cases)
     - [Investigative Coding](#investigative-coding)
     - [Research Coding](#research-coding)
   - [Code Debugging](#code-debugging)
   - [Code Review & Improvement](#code-review--improvement)
     - [Review Assistance](#review-assistance)
     - [Code Refinement](#code-refinement)
     - [Naming](#naming)
     - [Code Structure](#code-structure)
   - [Data Generation & Analysis](#data-generation--analysis)
     - [Data Generation](#data-generation)
     - [Data Analysis](#data-analysis)
5. [General-Purpose Applications (not necessarily development related)](#general-purpose-applications-not-necessarily-development-related)
   - [Summarization and Rephrasing](#summarization-and-rephrasing)
   - [Content Generation](#content-generation)
   - [Image Generation](#image-generation)
   - [Labelling](#labelling)
   - [Character Impersonation](#character-impersonation)
   - [Information Search](#information-search)
6. [Example on how I use it](#example-on-how-i-use-it)
   - [Authentication on WebSockets](#authentication-on-websockets)
   - [Evaluating Scalability of the WebSocket Implementation](#evaluating-scalability-of-the-websocket-implementation)
   - [Improvement ideas on code](#improvement-ideas-on-code)
   - [I need to implement something new](#i-need-to-implement-something-new)
   - [I want to see if I can improve upon something I developed](#i-want-to-see-if-i-can-improve-upon-something-i-developed)
7. [Prompt Examples](#prompt-examples)


## Recommended Tools

- **Cursor** / **Windsurf** – LLM-powered IDEs
- **GitHub Copilot** – Use with Agentic Mode for extended capabilities
- **Perplexity** – For AI-assisted search and technical research ([perplexity.ai](https://www.perplexity.ai/))
- **ChatGPT** – For general conversational interactions ([chatgpt.com](https://chatgpt.com/))
- **CodeRabbit** - For code reviews ([CodeRabbit](https://www.coderabbit.ai/))
- **WisprFlow.ai** - For speech-to-text so you can create prompts faster ([WisprFlow.ai](https://wisprflow.ai/)) (alternatives to this are OpenAI ChatGPT Client Desktop / Web and you can copy the result)

## Best Practices

- **Verify, do not trust**: The LLM output must be verified and not blindly trusted
- **Offer clear context**: Give the LLM context that you feel would be needed to a real person, if you would ask him to do this solution. Otherwise, if the LLM does not have access to that information via other tools (e.g.: is able to search the codebase), he would provide garbage.
- **Plan First**: Ask the LLM first for a plan of any changes he wants to propose, this makes him reason and think about his approach and would give you a more qualitive output.
- **Context Degradation**: As context accumulates, LLM performance typically degrades. Start a new session when output quality drops. Summarize relevant details from prior sessions before continuing.
- **Context Formatting**: Clearly delineate input using appropriate markers (`triple backticks`, XML-style tags, etc.), tailored to the LLM’s parsing preferences.
- **Supply Relevant Context**: If you want to create something specific, give him one or more examples of the desired result.

## LLM Use Cases in Development

### Feature Development

#### Large-Scale Features

Attempting to generate full large-scale features in a single prompt often yields poor results. Instead:

- Making think of a plan for the solution first before acting to do any changes
- Review said plan and consider if any changes to it are required.
- Alternatively, another approach is to make the LLM ask you any question he thinks might have regarding your solution (e.g.: "Ask any clarification question you feel it might be needed"). This not only helps the LLM, but it also helps you consider the edge-cases for a solution.
- Decompose features into smaller, modular tasks, separated by you.
- Treat each task independently, providing focused and relevant context.
- Follow the guidance outlined in **Small-Scale Feature Development**.

From my personal observations, it is often faster and more reliable to implement large features manually unless LLM context is tightly controlled.

#### Small-Scale Features

Effective when the change is **small** and **localized**. 
This includes testing something, like a feature or a class.

**Recommendations for context setup:**

- Include architecture rules or project structure documents.
- Provide the LLM IDE with relevant coding standards or linting rules.
- Add any related files (e.g., test files, DTOs, entities, utility functions) to the LLM’s context.
- Making think of a plan for the solution first before acting to do any changes

Failing to meet these requirements typically results in low-quality output.

### Exploratory Use Cases

#### Investigative Coding

Use LLMs to create **throwaway prototypes** or **implementation skeletons** when unsure about an approach. 
These can help trigger design ideas or architectural direction.
This includes generating new design for frontend application with no UX, so you can be inspired on how you want to present the data to the client.

#### Research Coding

For unknown features or technologies, LLMs can act as a first-pass researcher. Ask for:

- Initial implementation steps
- API usage examples
- Dependency suggestions (e.g.: What other libraries could be used to implement this solution)

**Note:** For research purposes it is better to use Perplexity, however for small and often well-known libraries a normal LLM is good enough.

**Follow-up considerations:**

- How secure is the solution?
- Is it scalable?
- Is error handling adequate?
- Should the design be more abstract or configurable?
- What alternatives exist, and how do they compare?

Continue the conversation with the LLM for deeper technical insights.

**If the code needed requires documentation that the LLM is not yet trained on, pass a link to the current documentation in the context**

### Code Debugging

Providing the LLM with sufficient context (e.g.: types, classes affected or potential places you think the error might originate from) can yield useful ideas or guidance for resolving an issue. 
However, it's essential to manually verify whether the suggested solution actually works.
Most importantly, you must understand the proposed solution—not just apply it blindly.

### Code Review & Improvement

#### Review Assistance

Use LLMs to review pull requests or code snippets. They often catch issues or offer suggestions you might overlook.

#### Code Refinement

LLMs are useful for improving isolated methods or functions. Prompts such as “refactor,” or “optimize,” often yield good results.

#### Naming

LLM can be useful when you are unsure how to name a certain variable, class or folder. This can either offer you a viable alternative solution or trigger an inspiration inside yourself.

#### Code Structure

LLM can help give you some hints or best practices ideas in how would you structure some certain folders you have in your application, where to best place them.
Consider giving him as context the existing structure (if it exists) and provide him with information regarding the framework you choose.

### Data Generation & Analysis

#### Data Generation
You can use an LLM to generate mock data for your application or simply create a factory function to generate said data.
To function properly, give the LLM as context a strict type of the data you want to generate (e.g.: a class, a type or a zod schema) with description.

#### Data Analysis
Providing the LLM with a certain dataset can help you receive some useful insights about it. 
However, be careful and double-check if the information provided to you is true by checking the actual data.

## General-Purpose Applications (not necessarily development related)

### Summarization and Rephrasing

LLMs excel at rewriting text. Enhance effectiveness by specifying:

- Tone: e.g., technical, concise, formal
- Format: bullet points, short summary, 600-character limit, etc.

**Example Prompt:**
```text
Rephrase the following message to be concise and technical. Limit to 600 characters:
```

### Content Generation

Use LLMs to generate structured content (e.g., documentation, user stories, emails) from minimal input. Prompt quality and structure heavily impact results.

### Image Generation

Leverage LLMs (with multimodal models or image generation tools) to create:

- Placeholder graphics
- Icons
- Logos
- UI mockups

### Labelling

The LLM can assist in labeling your dataset according to a specific set of rules. For example, it can help categorize items based on predefined criteria

### Character Impersonation

The LLM can impersonate a specific character—fantasy or otherwise—by understanding the character’s typical behavior or speech patterns. You can provide context or examples to guide its portrayal.
This is the basis for any virtual agent.

### Information Search

The LLM can assist in quickly finding information on a specific topic, as long as the information exists within its training data. For newer information, consider providing a link to the relevant documentation.
If the information needs to be gathered from multiple sources, or if you're unsure of the exact source and require high confidence in the quality of the answer, consider using Perplexity instead.

## Example on how I use it

#### Authentication on WebSockets
I implemented WebSockets in my backend and needed to restrict access to authenticated clients. I provided the LLM with details about my application's architecture, including the frameworks used on both the client and server sides, as well as the specific WebSocket library in use. Based on this context, it generated code snippets, which I adapted to fit my architecture.

Initially, I noticed that unauthenticated connections weren't being rejected during the handshake phase, but only after sending a message over the WebSocket channel. To address this, I used Perplexity to search for a solution and found useful reference links. After reviewing the resources, I decided on a new approach and asked the LLM to help generate a revised implementation. I then adapted the code accordingly and validated it through testing.

#### Evaluating Scalability of the WebSocket Implementation

As I prepared to deploy my WebSocket implementation to production, I wanted to assess whether it would scale effectively. The library I used offered optional extensions for scaling WebSocket connections, but it was unclear whether they were necessary in my setup.

I consulted the LLM, providing details about my cloud architecture. It initially recommended using the extensions. However, I had doubts and initiated a separate query in Perplexity, where I supplied more in-depth context about the infrastructure and message flow. This time, the response indicated the extensions were unnecessary. After reviewing the supporting reference links, I confirmed this assessment and decided not to adopt the proposed extensions.

#### Improvement ideas on code

I implemented a feature, but was unsatisfied with the code quality and felt it could be cleaner and more maintainable. I asked the LLM to suggest improvements. After reviewing the output, I evaluated whether it truly enhanced the original implementation. If it did, I incorporated the changes, adapting them to align with the project's coding standards.

#### I need to implement something new

I begin by asking the LLM to explain how a concept works, comparing its explanation with my existing knowledge. Then, I request a basic implementation to study and analyze. I follow up with targeted questions—such as clarifications around specific components, security concerns, or potential optimizations. To broaden my understanding, I use Perplexity to explore how the solution is typically implemented at an enterprise level. Based on this comparative analysis, I make an informed decision on the best approach.

#### I want to see if I can improve upon something I developed

I aim to minimize issues in my code and ensure nothing important has been overlooked. To support this, I ask an LLM to review my pull request—either by providing a `.patch` diff or using an LLM-integrated IDE like Cursor. I review the suggestions it provides, assess their relevance, and apply any necessary fixes accordingly.


## Prompt Examples

tba
