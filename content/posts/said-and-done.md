+++
title = "Journaling with AI: Said & Done"
date = "2025-08-26"

[taxonomies]
tags=["python", "llm", "ai"]

[extra]
repo_view = true
+++

# Journaling

## What is Journaling (briefly)

It's like a pragmatic diary where you write about your goals and what you want to achieve and things like that. It's a versatile, personal thing that takes many shapes or forms. Over the years I've had kept physical and digital journals, switching between them depending on my mood or whichever felt more convenient. Writing stuff eases the mind and tidy journaling gives me some data to keep track of activities like exercise and studies.

Until recently, I had no problems with writing. But now I find it **really** difficult and I can't find out why. That's why I decided to attack this problem from an angle that is unfamiliar to me: _speaking._

I am not good at speaking. I don't like tech that interacts with speech. I don't like using my voice unless I have to. Unfortunately, this is suboptimal. _Now_ is a good time to change this for the better as writing is unattractive and speaking about my day doesn't sound too bad. I could even put a speech-to-text model and let an LLM extract the information and generate markdown files- wait, that's not a bad idea at all!

# AI could be good at this

## Idea

I want to talk about my day, giving various information like; when I emerge from bed, what programming project I work on, exercises like running or cycling; or more mundane activities like chores, cooking, or how my pour over coffee turned out. AI could extract this information because my range of activities are limited so it won't see something unusual outside this category. This information should be logged to a markdown file as a journal to make it easy to read and to correct and add additional notes if needed. It could also be stored in a database for further RAG stuff or analysis. Additionally, I can load previous days to the prompt to give AI context about my previous days so it can remind me about things I neglected or praise me for my diligent work.

The last bit sounds funny but it has potential to be very useful to me. It would be great to be reminded about the dessert I made last week to give me chance to make it again, to be reminded of a programming project I was shying away from because of a roadblock, to be reminded of chores to keep the house tidy and clean and to be reminded to go outside to touch grass and socialize.

## AI things

This project requires AI for the following features:

- Speech to text
- Extracting information from transcription
- Generating markdown files
- Feedback for the next day

### Speech to text

After some reading online, I was guided to OpenAI's open source Whisper models. _Insert a joke about OpenAI having open source models after so long, despite having open in their names_.

Base model for English works great for it's size and resource requirements. The turbo? model is amazing, but it wants more juice. I decided to go with base model because not everyone has 8GBs of VRAM and I didn't want to rely too much on an amazing model. The base model needs a slow and accurate speaking though.

Whisper's python package broke `uv` which is a shame though.

### Extraction

Extraction is honestly incredible. It opens so much possibilities. It's one of the key parts of this project as well.

I followed `langchain`'s tutorial on extraction and then recommended practices. Giving extraction examples improved the accuracy and since then I have not had any false extractions.

_Give the "I didn't do any sports" example_

### Generating markdown files

Giving examples here also helped. Honestly, the biggest issue was finding a journal format that I was pleased with.

### Feedback

Writing the prompt for feedback is tricky. I had to steer it to the direction I wanted, otherwise it says useless things.

### Langchain and Langgraph

I had no experience with any of this when I started, I only heard of langchain a few times and browsed their docs a bit. At first I coded everything in langchain but it was confusing and a bit ugly so I refactored it to langgraph and I am satisfied with the results. It's significantly easier to reason about it.

_Mermaid plot here_

### LLM Model

I used Google GenAI for this project. It works well and it has a generous free API. I want to have the option to switch to a local one but I don't have disk space for local models at the moment. Also adding configuration options for other models would be great.

## Non-AI things

### Structure

Structuring a Python project with a lot of moving parts is something I had no experience with. I went ballistic to gain that experience and I refactored the whole codebase at least 4 times. Python is tricky because it works anyway and makes it harder to judge right or wrong. This is suboptimal when my goal is to learn and learn good. I followed my made up metric of "gud enuf" and stopped when I felt it was "gud enuf".

There's a cool trick with decorators I learned from windows-screen-capture library. It allows a good separation for functions that handle some internal thing provided by library.
