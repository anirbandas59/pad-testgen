# PAD Test Case Generator

**PAD Test Case Generator** turns a Blue Prism 7.3 release package (`.bprelease`) into an Excel test
script in the client test template (Instructions, Test Cases and Defects sheets). Testers use the
script in SIT and UAT of the process's migration to Power Automate.

This repository holds the **releases only**. Download the edition you want from
[Releases](../../releases).

## Choose an edition

| Edition | Release tags | For | You need |
|---|---|---|---|
| **CLI** | `cli-vX.Y.Z` | Scripted or repeated runs; CI; people at home in a terminal | Python 3.14 and [uv](https://docs.astral.sh/uv/) |
| **UI** | `ui-vX.Y.Z` | Guided, point-and-click runs | See the UI release notes |

Both editions use the same generator. Each release says which generator version it contains.

## CLI edition: install and run

1. Download `pad_test_case_generator-<version>-py3-none-any.whl` from the latest `cli-v…` release.
2. Install it:

   ```bash
   uv tool install ./pad_test_case_generator-<version>-py3-none-any.whl
   pad-test-case-generator --help
   ```

3. Run it on a release package:

   ```bash
   pad-test-case-generator list-processes path/to/release.bprelease
   pad-test-case-generator run path/to/release.bprelease --process "My Process"
   ```

   The workbook is written to `runs/<Process>/<hash>/`.

The source archive in each release holds the full README, including how `run` and `resume`
work, the LLM options and configuration.

## Choosing an LLM, and your data

The generator writes the test cases with an LLM. The structure, numbering and coverage checks are
done by code.

- **`--ai manual` (the default)** keeps everything local. It writes a request bundle that you can
  answer with any AI you choose, then `resume` the run.
- **An API service** (OpenAI, Anthropic, Google Gemini, or a local Ollama) needs a services
  catalogue. The wheel does not include one; without it only `manual` is available. Copy
  `ai_services.json` and `.env.example` from the release's source archive into the folder you run
  `pad-test-case-generator` from:
  - set a `model` for each service you want to use in `ai_services.json`;
  - rename `.env.example` to `.env` and fill in your API keys there (or set them as environment
    variables);
  - run with `--ai <service>`, for example `--ai openai`.

  In this mode, **redacted prompts are sent to that service.** Hard-coded credential values and
  environment-variable values are always removed first. For sensitive client data, use `manual`
  or a purely local model.

## Licence

Proprietary: free to download and use, unmodified. It may not be redistributed or modified. The
exception is configuration: you may copy and edit `ai_services.json` to use your own AI services,
and copy `.env.example` to local `.env` files that hold your own API keys. See `LICENSE` in each
release.

## Issues

Report problems in [Issues](../../issues). Say which edition and version you used. Never attach a
client's release package or workbook.
