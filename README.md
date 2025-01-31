# openai-assistants-cli

Command-line interface for ChatGPT Assistants: https://platform.openai.com/docs/assistants/overview.


## Features

- **Command-Line Interface**: Interact with ChatGPT or Claude directly from your terminal.
- **Multiple Assistants**: Easily switch between different assistants defined in the config file.
- **Keyboard Shortcuts**: Use Ctrl-C, Ctrl-D, and Ctrl-R shortcuts for easier conversation management and input control.
- **Multi-Line Input**: Enter multi-line mode for more complex queries or conversations.
- **Markdown Support**: Enable or disable markdown formatting for chat sessions to tailor the output to your preferences.
- **Logging**: Log your conversations to disk or stdout

### Coming Soon
- **Usage tracking**: Track your API usage with token count and price information.

## Installation

This install assumes a Linux/OSX machine with Python and pip available.
```bash
pip install openai-assistants-cli
```

Install latest version from source:
```bash
pip install git+https://github.com/grid-link-inc/gpt-cli
```

Or install by cloning the repository manually:
```bash
git clone https://github.com/grid-link-inc/gpt-cli
cd gpt-cli
pip install .
```

Add the OpenAI API key to your `.bashrc` file (in the root of your home folder).
In this example we use nano, you can use any text editor.

```
nano ~/.bashrc
export OPENAI_API_KEY=<your_key_here>
```

Run the tool

```
openai-assistants-cli
```

You can also use a `gpt.yml` file for configuration. See the [Configuration](README.md#Configuration) section below.

## Usage

Make sure to set the `OPENAI_API_KEY` environment variable to your OpenAI API key (or put it in the `~/.config/gpt-cli/gpt.yml` file as described below).

Add your assistant to the config file. See `Configuration` below. (TODO Add assistant-id arg and use this in Usage/quickstart)

```
usage: openai-assistants-cli [-h] [--no_markdown] 
              [--log_level {DEBUG,INFO,WARNING,ERROR,CRITICAL}]
              [--no_stream]
              [{assistant-name}]

Run a chat session with ChatGPT. See https://github.com/grid-link-inc/gpt-cli for more information.

optional arguments:
  -h, --help            show this help message and exit
  --no_markdown         Disable markdown formatting in the chat session.
  --log_level {DEBUG,INFO,WARNING,ERROR,CRITICAL}
                        The log level to use
  --no_stream           If specified, will not stream the response to standard output. This is
                        useful if you want to use the response in a script. Ignored when the
                        --prompt option is not specified.
  --no_price            Disable price logging.
```

Type `:q` or Ctrl-D to exit, `:c` or Ctrl-C to clear the conversation, `:r` or Ctrl-R to re-generate the last response.
To enter multi-line mode, enter a backslash `\` followed by a new line. Exit the multi-line mode by pressing ESC and then Enter.


## Configuration

You can configure the assistants in the config file `~/.config/gpt-cli/gpt.yml`. The file is a YAML file with the following structure (see also [config.py](./gptcli/config.py))

```yaml
default_assistant: <assistant_name>
markdown: False
openai_api_key: <openai_api_key>
anthropic_api_key: <anthropic_api_key>
log_file: <path>
log_level: <DEBUG|INFO|WARNING|ERROR|CRITICAL>
assistants:
  <assistant_name>:
    id: <assistant id string>
  <assistant_name>:
    ...
```

You can override the parameters for the pre-defined assistants as well.

You can specify the default assistant to use by setting the `default_assistant` field. 

Example:

```yaml
default_assistant: my_assistant
markdown: True
openai_api_key: <openai_api_key>
assistants:
  my_assistant:
    id: asst_abcdabcdabcd
  my_other_assistant:
    id: asst_123412341234
```

```
$ openai-assistants-cli my_other_assistant

> 
```


## Testing

```
pytest tests
```


# TODO for v1.0

- [ ] Implement PriceChatListener for Assistants
- [ ] Add configurable instructions for each assistant to be passed to runs.create()
- [ ] Accept an assistant ID as a cli arg
- [ ] Make file names clickable in citations (that were created by add_citations_to_messages()) 
- [ ] Cache File retrievals
- [ ] Consolidate the two log file / persistance implementations

# Publishing

```
## Build
pip install build
python -m build

## Test it
pip install twine
twine upload --repository testpypi dist/* --username __token__ # Use your API token as the password when promtped
pip install --index-url https://test.pypi.org/simple/ openai-assistants-cli

## Publish
twine upload dist/* --username __token__ # Use your API token as the password when promtped
```