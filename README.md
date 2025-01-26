# Notes and Retrospective on AI-Assisted Backend Development, Oct 2024

## Project Context

This repository contains my retrospective from building a backend REST API as a take-home interview project in October 2024. While I can't share the code for confidentiality reasons, I want to share my learnings about AI-assisted development.

The project used FastAPI, Pydantic, SQLAlchemy, and PostgreSQL to implement a REST API with a one-to-many database relationship pattern. I included a docker-compose deployment for easy testing with curl.

## AI Assistance for Development

Inspired by OpenAI's [o1-preview release](https://openai.com/index/introducing-openai-o1-preview/) and this [podcast episode](https://newsletter.pragmaticengineer.com/p/ai-tools-for-software-engineers-simon-willison) with Gergely Orosz and Simon Willison, I used ChatGPT with o1-preview to generate the initial code. I treated it like an intern on a project where I'm familiar with the stack, reviewing the code and iterating with feedback prompts.

The results were excellent - the generated code mostly ran out of the box and produced boilerplate and test cases faster than manual writing. Notable findings from my half-day of testing and updates:

- Module import paths needed fixes
- In-memory SQLite in unit tests failed with multiple db sessions
- A performance bug where Pydantic-SQLAlchemy interaction caused unnecessary joins
- Use of deprecated Pydantic V2 methods (expected since LLMs train on historical data)
- Deployment notes: Mac Control Center can bind to port 5000, curl needs `-L` for 307 redirects

GitHub Copilot in VS Code also proved helpful during manual changes.

## Development Workflow Friction

The main inefficiency was context switching between ChatGPT's UI and VS Code. I initially delayed running the code, doing several review rounds in ChatGPT first (like a GitHub PR review). Starting testing earlier would have been more efficient.

I'm now exploring Cursor, a VS Code fork with integrated AI chat that can directly edit code. While ChatGPT with Canvas offers similar capabilities, tools like Cursor may better suit larger projects. I plan to validate this hypothesis in future work.

## Framework Insights

While Python, SQLAlchemy, PostgreSQL, and Docker were familiar territory, FastAPI and Pydantic proved to be meaningful upgrades from Flask. Despite some extra debugging time due to less familiarity, I'd choose FastAPI for new Python API projects unless specific constraints suggest otherwise.

As an experiment, I had ChatGPT port the application to Flask, Go, Node.js, and even generate React frontend and iOS app demos. While untested, the reasonable-looking output demonstrates AI's potential for rapid prototyping across platforms.
