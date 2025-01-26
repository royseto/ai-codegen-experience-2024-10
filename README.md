# Notes and Retrospective on AI-Assisted Backend Development

## Project Context

This repository contains my notes and retrospective from implementing a backend REST API project in October 2024. I can't share the project itself for confidentiality reasons because it was a take-home project for a company I was interviewing with.

The project involved designing and implementing a persistence layer and REST API in Python, with endpoints that could be tested locally using curl. The implementation used FastAPI, Pydantic, SQLAlchemy, and PostgreSQL to handle database entities with a one-to-many relationship pattern common in backend systems. I included a deployment script for running the app with docker-compose.

## AI Assistance for Development

I'd been experimenting with AI-assisted development throughout 2024. Inspired by OpenAI's [o1-preview release](https://openai.com/index/introducing-openai-o1-preview/) and this [podcast episode](https://newsletter.pragmaticengineer.com/p/ai-tools-for-software-engineers-simon-willison) with Gergely Orosz and Simon Willison, I tried using ChatGPT with the o1-preview model to generate the initial code for this project. I treated it like an intern on a project where I am familiar with the stack. I reviewed the generated code and iterated with more prompts in o1-mini to give ChatGPT code review feedback.

Results of this process were excellent. Other than needing to fix up a bug in module import paths, the generated code mostly ran out of the box. It generated boilerplate code and unit test cases faster than I would do it manually.

I spent about half a day testing and updating the code to production standards. Here are some notable learnings from reviewing and fixing the AI-generated code:

- Module import path issues needed manual fixes
- The generated code used in-memory SQLite for unit tests, which didn't handle multiple db sessions in test fixtures correctly. This took about an hour to debug and fix.
- I found a performance bug where the generated code was doing unnecessary database joins due to how the generated Pydantic model interacted with SQLAlchemy ORM's automatic relationship loading. This is a pattern to watch for in code review.
- The LLM generated code used deprecated Pydantic V2 methods (not surprising since LLMs train on historical data). I updated these to current recommendations.
- Through manual testing, I discovered some practical deployment considerations: Mac Control Center sometimes binds to port 5000, and curl requires the `-L` flag to follow 307 redirects.

I also used GitHub Copilot in VS Code when making manual changes. Its suggestions were helpful in speeding up the work.

## Friction Points I'd Like to Improve

When I started running and testing the generated code, I needed to fix a few issues. I did this in VS Code after pasting the code there from ChatGPT's UI. Then I wrote detailed prompts feeding the changes back to ChatGPT. This worked, but it was cumbersome. 

Also, to delay dealing with this inefficiency, I didn't run the code for the first time until I had done several rounds of code review in the ChatGPT UI, similar to how I would review a GitHub pull request. It would have been faster overall to start testing the code sooner. Making the integration between running the code and asking the AI to apply more prompts to it tighter would remove this friction.

I just started trying Cursor, a fork of VS Code that adds (among other AI features) a chat interface that can apply edits to the code in the IDE using natural language. It seems promising to solve this friction point. I'd like to try building a similar project with Cursor to validate this.

I can also see how ChatGPT with o1 and Canvas could do something similar, though I would expect it to be limited in the size of project it could handle without becoming cumbersome. I would expect something like Cursor to be a better fit for most commercial projects beyond prototyping for this reason, though I haven't validated this hypothesis yet.

## Retrospective on FastAPI and Pydantic

I've used Python, SQLAlchemy, PostgreSQL, and Docker in production for many years. These parts of the stack held no surprises. 

FastAPI and Pydantic are an upgrade from Flask, which I used in production previously. I've built a couple small tutorial/personal projects with FastAPI in 2024 and chose to use it for this project. I think the end result is very good and would choose FastAPI for new API projects in Python unless there is a strong reason to use another framework. However, I did spend some extra debugging time since I was less familiar with FastAPI than Flask. 

As an experiment, I prompted ChatGPT to port the application to Flask, Go, and Node.js, as well as to design and implement both a React frontend and a native iPhone app to demo the endpoints. It generated reasonable-looking code for all these variations in minutes, though I didn't test the output. This demonstrates the potential of AI tools to quickly explore different technical approaches and generate prototype code across multiple platforms.
