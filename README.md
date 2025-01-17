<div align="center">
  <h1>gpt-code-search</h1>
  <img
    height="240"
    width="240"
    alt="logo"
    src="https://raw.githubusercontent.com/wolfia-app/gpt-code-search/main/public/logo.png"
  />
  <p>
    <b>gpt-code-search</b> is a tool enabling you to search your codebase with natural language. It utilizes OpenAI's function calling to retrieve, search and answer queries about your code, boosting productivity and code understanding.
  </p>
</div>

Learn more about the motivation behind this project in our announcement [blog post](https://wolfia.com/blog/announcing-gpt-code-search).

## Features

- 🧠 **GPT-4**: Code search, retrieval, and answering all done with OpenAI's [function calling](https://openai.com/blog/function-calling-and-other-api-updates).
- 🔐 **Privacy-first**: Code snippets only leave your machine when you ask a question and the LLM requests the relevant code.
- 🔥 **Works instantly**: No pre-processing, chunking, or indexing, get started right away.
- 📦 **File-system backed**: Works with any code on your machine.

## Getting Started

### Installation

```bash
pip install gpt-code-search
```

### Usage

#### Ask a question about your codebase

To query about the purpose of your codebase, you can use the `query` command:

```bash
gpt-code-search query "What does this codebase do?"
# or use the shorthand alias
gcs query "What does this codebase do?"
```

<img src="public/demo.gif" width="750"  alt="gpt-code-search demo"/>

If you want to generate a test for a specific file, for example analytics.py, you can mention the file name to improve accuracy:
```bash
gcs query "Can you generate a test for analytics.py?"
```

For a general usage question about a certain module, like analytics, you can use keywords to search across the codebase:
```bash
gcs query "How do I use the analytics module?"
```

**Remember, mentioning the file name or specific keywords improves the accuracy of the search.**

#### Select a model to use

```bash
gcs select-model
```

Defaults to `gpt-3.5-turbo-16k`. The selected model is stored in `$HOME/.gpt-code-search/config.toml`.


### Configuration

The tool will prompt you to configure the `OPENAI_API_KEY`, if you haven't already.

## Problem

You want to leverage the power of GPT-4 to search your codebase, but you don't want to manually copy and paste code snippets into a prompt nor send your code to another third-party service (other than OpenAI).

This tool solves these problems by letting GPT-4 determine the most relevant code snippets within your codebase. Also, it meets you where you already live, in your terminal, not a new UI or window.

Examples of the types of questions you might want to ask:

- 🐛 Help debugging errors and finding the relevant code and files
- 📝 Document large files or functionalities formatted as markdown
- 🛠️ Generate new code based on existing files and conventions
- 📨 Ask general questions about any part of the codebase

## How it works

We utilize OpenAI's function calling to let GPT-4 call certain predefined functions in our library. You do not need to implement any of these functions yourself. These functions are designed to interact with your codebase and return enough context for the LLM to perform code searches without pre-indexing it or uploading your repo to a third party other than OpenAI. So, you only need to run the tool from the directory you want to search.

<img src="public/architecture.png" width="650" />

The functions currently available for the LLM to call are:

- `search_codebase` - searches the codebase using a TF-IDF vectorizer
- `get_file_tree` - provides the file tree of the codebase
- `get_file_contents` - provides the contents of a file

These functions are implemented in `gpt-code-search` and are triggered by chat completions. The LLM is prompted to utilize the search_codebase and get_file_tree function as needed to find the necessary context to answer your query and then loops as needed to collect more context with the get_file_contents until the LLM responds.

### Privacy

This tool prioritizes privacy. Outside of the LLM, no code is sent to us and is only used as context for the LLM. We do collect anonymous usage data to improve the tool, but you can opt out of this.

## Limitations

This does have some limitations, namely:

- The LLM is unable to load context across multiple files at once. This means that if you ask a question that requires context from multiple files, you will need to ask multiple questions.
- Specify the file name and keywords in your question to improve accuracy. For example, if you want to ask a question about `analytics.py`, mention the file name in your question.
- The level of search and retrieval is limited by the context window, which refers to the scope of the search conducted by the tool, meaning that we can only search 5 levels deep in the file system. So you need to run the tool from the folder/package closest to the code you want to search.

These limitations lead to suboptimal results in a few cases, but we're working on improving this. **We wanted to get this tool out there as soon as possible to get feedback and iterate on it!**

## Roadmap

- [ ] Use vector embeddings to improve search and retrieval
- [ ] Add support for generating code and saving it to a file
- [ ] Support for searching across multiple codebases
- [ ] Allow the model to create new functions that it can then execute
- [ ] Use [guidance](https://github.com/microsoft/guidance) to improve prompts
- [ ] Add support for additional models (Claude, Bedrock, etc)

## Wolfia Codex

`gpt-code-search` is a simplified version of [Wolfia Codex](https://wolfia.com), a cloud tool that enables you to ask any question about open source and private code bases like [`Langchain`](https://wolfia.com/?projectId=2b964031-0ce8-472a-abb7-27079a7b84f3), [`Vercel ai`](https://wolfia.com/?projectId=4710df1f-43f8-4d30-863b-d67876ae0f06), or [`gpt-engineer`](https://wolfia.com/?projectId=8d9dd449-da2d-410e-a4fc-f2ff75a30f73).

If you're looking for a more powerful tool which solves the above limitations by using vector embeddings and a more powerful search and retrieval system, or avoiding the setup, check out [Wolfia Codex](https://wolfia.com), search codebases, share your questions and answers, and more!

## Analytics

We collect anonymous crash and usage data to help us improve the tool. This data aids in understanding usage patterns and improving the tool. You can opt out of analytics by running:

```bash
gcs opt-out-of-analytics
```

You can check the data that by looking at the [analytics](core/analytics.py) and [config](core/config.py) files.

Here's an exhaustive list of the data we collect:

```
- exception - stacktraces of crashes
- uuid - a unique identifier for the user
- model - the model used for the query
- usage - the type of usage (query_count, query_at, query_execution_time)
```

**Note: We do not collect any PII (ip-address), queries or code snippets.**

## Contributing

We love contributions from the community! ❤️ If you'd like to contribute, feel free to fork the repository and submit a pull request.

Please read our [Code of Conduct](CODE_OF_CONDUCT.md) and [Contributing Guide](CONTRIBUTING.md) for more detailed steps and information.

## Code of Conduct

We are committed to fostering a welcoming community. To ensure that everyone feels safe and welcome, we have a [Code of Conduct](CODE_OF_CONDUCT.md) that all contributors, maintainers, and users of this project are expected to adhere to.

## Support

If you're having trouble using `gpt-code-search`, feel free to [open an issue](https://github.com/wolfia-app/gpt-code-search/issues) on our GitHub. You can also reach out to us directly at [support@wolfia.com](mailto:support@wolfia.com). We're always happy to help!

## Feedback

Your feedback is very important to us! If you have ideas for how we can improve `gpt-code-search`, we'd love to hear from you. Please [open an issue](https://github.com/wolfia-app/gpt-code-search/issues) with your suggestions, or you can email [support@wolfia.com](mailto:support@wolfia.com).

## License

Apache 2.0 © [Wolfia](https://wolfia.com)
