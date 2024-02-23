# OpenAI PR Description GitHub Action

Autofill the description of your pull requests with the power of OpenAI.

## What does it do?

`EDGLRD/openai-pr-description` is a GitHub Action that looks at the title as well as the contents
of your pull request and uses the [OpenAI API](https://openai.com/blog/openai-api) to generate the description of your pull request.

The GitHub Action will only run when a PR description is not already provided.
In other words it will not accidentally overwrite your existing description.
The idea is this Action will save you the time and trouble of writing **meaningful** pull request descriptions.

**NOTE: You still must manually review and modify the PR description before submitting!**

For information on customizing OpenAI variables as an admin, refer to the [Customizing OpenAI variables (Admin only)](#customizing-openai-variables-admin-only) section.

The cost is around ~$0.10 for 15-20 pull requests when using gpt-3.5.

## How can you use it?

1. Create an account on OpenAI, set up a payment method and get your [OpenAI API key].
2. Add the OpenAI API key as a [GitHub Secret](https://docs.github.com/en/actions/security-guides/encrypted-secrets) in your repository's settings.
3. Create a workflow YAML file, e.g. `.github/workflows/openai-pr-description.yml` with the following contents:

```yaml
name: Generate PR description

on: pull_request

jobs:
  openai-pr-description:
    runs-on: ubuntu-22.04

    steps:
      - uses: EDGLRD/openai-pr-description@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          openai_api_key: ${{ secrets.OPENAI_API_KEY }}
```

## Variables

### Static

| Input             | Description                                           |
| ----------------- | ----------------------------------------------------- |
| `github_token`    | The GitHub token to use for the Action                |
| `openai_api_key`  | The [OpenAI API key](https://help.openai.com/en/articles/4936850-where-do-i-find-my-secret-api-key) to use    |
| `pull_request_id` | The ID of the pull request to use                     |

### Customizeable

| Input             | Description                                           | Default                    |
| ----------------- | ----------------------------------------------------- | -------------------------- |
| `openai_model`    | The [OpenAI model](https://platform.openai.com/docs/models) to use                             | `gpt-3.5-turbo`            |
| `max_tokens`      | The maximum number of **prompt tokens** to use        | `1000`                     |
| `temperature`     | Higher values will make the model more creative (0-2) | `1.0`                      |
| `system_prompt`   | The high-level system prompt to use for giving context to the model     | See [action.yml](action.yml)             |
| `sample_prompt`   | The user prompt to use for giving context to the model     | See [action.yml](action.yml)             |
| `sample_response` | A assistant response for giving context to the model     | See [action.yml](action.yml)             |

#### Customizing OpenAI variables (Admin only)

1. Go to Organization (to set for all repositories using the action) or Repository (for a specific repository) settings on GitHub
2. In `Secrets and variables`, create a new variable (e.g. `INPUT_SYSTEM_PROMPT` for `system_prompt`) and enter a new value
3. The new value will be used instead of the default defined in action.yml

## `403` error when updating the PR description

If you get a `403` error when trying to update the PR description, it's most likely because
the GitHub Action is not allowed to do so.
The easiest way forward is to grant the necessary permissions to the `GITHUB_TOKEN` secret
at `<your_repo_url>/settings/actions` under `Workflow permissions`.
