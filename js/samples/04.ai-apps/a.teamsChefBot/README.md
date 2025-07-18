# Microsoft Teams Conversational Bot with AI: Teams Chef

This is a conversational bot for Microsoft Teams that thinks it's a Chef to help you cook apps using the Teams AI Library. The bot uses the `gpt-4o` model to chat with Teams users and respond in a polite and respectful manner, staying within the scope of the conversation.

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Microsoft Teams Conversational Bot with AI: Teams Chef](#microsoft-teams-conversational-bot-with-ai-teams-chef)
    - [Summary](#summary)
    - [Setting up the sample](#setting-up-the-sample)
    - [Testing the sample](#testing-the-sample)
        - [Using Microsoft 365 Agents Toolkit for Visual Studio Code](#using-teams-toolkit-for-visual-studio-code)
        - [Using Microsoft 365 Agents playground](#using-teams-app-test-tool)

<!-- /code_chunk_output -->

## Summary

This sample illustrates how to use [Retrieval Augmented Generation (RAG)](https://en.wikipedia.org/wiki/Prompt_engineering#Retrieval-augmented_generation) to easily inject contextual relevant information into the prompt sent to the model. This results in better and more accurate replies from the bot.

The sample uses a local Vector Database, called [Vectra](https://github.com/Stevenic/vectra), and [Semantic Search](https://en.wikipedia.org/wiki/Semantic_search) to find the most relevant information to include in the prompt for the users input. The index can be found in `./index/teams-ai` and includes all of the projects Getting Started docs and the source code for the Teams AI Library. This means you can ask the Teams Chef Bot anything about the library and it can answer it. You can even ask it to write sample code for you!

![Teams Chef Bot](./assets/TeamsChef003.png?raw=1)

## Setting up the sample

1. Clone the repository

```bash
git clone https://github.com/Microsoft/teams-ai.git
```

> [!IMPORTANT]
> To prevent issues when installing dependencies after cloning the repo, copy or move the sample directory to it's own location first.
> If you opened this sample from the Sample Gallery in Microsoft 365 Agents Toolkit, you can skip to step 2.

```bash
cd path/to/04.ai-apps/a.teamsChefBot
code .
```

2. Add your OpenAI key to the `SECRET_OPENAI_KEY` variable in the `./env/.env.local.user` file and comment out lines 90-93 in `./src/index.ts` file.

If you are using Azure OpenAI then follow these steps:

- Comment the `SECRET_OPENAI_KEY` variable in the `./env/.env.local.user` file.
- Add your Azure OpenAI key and endpoint values to the `SECRET_AZURE_OPENAI_KEY` and `SECRET_AZURE_OPENAI_ENDPOINT` variables.
- Open the `m365agents.local.yml` file and modify the last step to use Azure OpenAI variables instead:

```yml
- uses: file/createOrUpdateEnvironmentFile
    with:
      target: ./.env
      envs:
        BOT_ID: ${{BOT_ID}}
        BOT_PASSWORD: ${{SECRET_BOT_PASSWORD}}
        #OPENAI_KEY: ${{SECRET_OPENAI_KEY}}
        AZURE_OPENAI_KEY: ${{SECRET_AZURE_OPENAI_KEY}}
        AZURE_OPENAI_ENDPOINT: ${{SECRET_AZURE_OPENAI_ENDPOINT}}
```

- Open `./infra/azure.bicep` and comment out lines of 'OPENAI_KEY' section and uncomment lines of 'AZURE_OPENAI_KEY' and 'AZURE_OPENAI_ENDPOINT' sections.
- Open `./infra/azure.parameters.json` and replace lines of "openAIKey" section with:

```json
      "azureOpenAIKey": {
        "value": "${{SECRET_AZURE_OPENAI_KEY}}"
      },
      "azureOpenAIEndpoint": {
        "value": "${{SECRET_AZURE_OPENAI_ENDPOINT}}"
      }
```

> Please note: If you are using Azure OpenAI, you will need both a GPT model and embedding model deployment to get this sample working. See `OpenAIModel` in `index.ts` and `OpenAIEmbeddings` in `VectraDataSource.ts`. [Learn more on this community post - How To Set Up and Configure a GPT Deployment Using the Azure OpenAI Service](https://techcommunity.microsoft.com/blog/startupsatmicrosoftblog/how-to-set-up-and-configure-a-gpt-deployment-using-the-azure-openai-service/3849854)

3. Update `./src/prompts/chat/config.json` and `./src/index.ts` with your model deployment name.

## Testing the sample

The easiest and fastest way to get up and running is with Microsoft 365 Agents Toolkit as your development guide. To use Microsoft 365 Agents Toolkit to automate setup and debugging, please [continue below](#using-teams-toolkit-for-visual-studio-code).

Otherwise, if you only want to run the bot locally and build manually, please jump to the [BotFramework Emulator](https://github.com/microsoft/teams-ai/blob/main/getting-started/OTHER/BOTFRAMEWORK-EMULATOR.md) section.
For different ways to test a sample see: [Multiple ways to test](https://github.com/microsoft/teams-ai/tree/main/getting-started/OTHER)

### Using Microsoft 365 Agents Toolkit for Visual Studio Code

The simplest way to run this sample in Teams is to use Microsoft 365 Agents Toolkit for Visual Studio Code.

1. Ensure you have downloaded and installed [Visual Studio Code](https://code.visualstudio.com/docs/setup/setup-overview)
1. Install the [Microsoft 365 Agents Toolkit extension](https://marketplace.visualstudio.com/items?itemName=TeamsDevApp.ms-teams-vscode-extension)
1. Copy this sample into a new folder outside of teams-ai
1. Select File > Open Folder in VS Code and choose this sample's directory
1. Using the extension, sign in with your Microsoft 365 account where you have permissions to upload custom apps
1. Verify that the Microsoft 365 Agents Toolkit extension is connected to your Teams account from the above step.
1. Select **Debug > Start Debugging** or **F5** to run the app in a Teams web client.
1. In the browser that launches, select the **Add** button to install the app to Teams.

> If you do not have permission to upload custom apps (sideloading), Microsoft 365 Agents Toolkit will recommend creating and using a Microsoft 365 Developer Program account - a free program to get your own dev environment sandbox that includes Teams.

### Using Microsoft 365 Agents playground

If you are using Azure OpenAI then follow these steps:

- Comment the `SECRET_OPENAI_KEY` variable in the `./env/.env.playground` file.
- Add your Azure OpenAI key and endpoint values to the `SECRET_AZURE_OPENAI_KEY` and `SECRET_AZURE_OPENAI_ENDPOINT` variables
- Open the `m365agents.playground.yml` file and modify the last step to use Azure OpenAI variables instead:

```yml
- uses: file/createOrUpdateEnvironmentFile
  with:
      target: ./.localConfigs.playground
      envs:
          TEAMSFX_NOTIFICATION_STORE_FILENAME: ${{TEAMSFX_NOTIFICATION_STORE_FILENAME}}
          # OPENAI_KEY: ${{SECRET_OPENAI_KEY}}
          AZURE_OPENAI_KEY: ${{SECRET_AZURE_OPENAI_KEY}}
          AZURE_OPENAI_ENDPOINT: ${{SECRET_AZURE_OPENAI_ENDPOINT}}
```

1. Ensure you have downloaded and installed [Visual Studio Code](https://code.visualstudio.com/docs/setup/setup-overview)
1. Install the [Microsoft 365 Agents Toolkit extension](https://marketplace.visualstudio.com/items?itemName=TeamsDevApp.ms-teams-vscode-extension)
1. Copy this sample into a new folder outside of teams-ai
1. Select File > Open Folder in VS Code and choose this sample's directory
1. From the left pane, select **Run and Debug**(Ctrl+Shift+D) and select **Debug in Microsoft 365 Agents playground** in dropdown list.
1. Select Debug > Start Debugging or F5 to run the app.
1. The browser will pop up to open Microsoft 365 Agents playground.
