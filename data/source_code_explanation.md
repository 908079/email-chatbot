# Deep Dive into Source Code

## Introduction
The provided script is for a Streamlit application named "Smart Mailbox Assistant". The assistant uses artificial intelligence (AI) to interact with users in a chat-like interface, answer their queries, and accept feedback. The assistant communicates with Gmail using Zapier and employs OpenAI's language model to generate human-like responses.

## Imports
The script begins by importing various libraries and modules:

* **streamlit**: The main library for creating the web application.
* **streamlit_chat:** A library to help create a chat interface in Streamlit.
* **ZapierToolkit:** Classes that provide functions for interacting with the user Gmail account through Zapier.
* **OpenAI:** A class that uses OpenAI's language model.
* **initialize_agent, AgentType, ZeroShotAgent, AgentExecutor:** Classes related to the AI agent creation and execution.
* **ConversationBufferMemory:** A class for managing the conversation history.
* **LLMChain:** A class for chaining language model operations.
* **ZapierNLAWrapper:** A class for wrapping Zapier's Natural Language Actions (NLA).
* **MessagesPlaceholder:** A placeholder class for messages.
* **requests, HTTPBasicAuth:** Libraries for making HTTP requests.
* **urlparse, parse_qs:** Functions for parsing URLs.
* **SystemMessage, HumanMessage, AIMessage:** Classes representing different types of messages.
* **os:** Standard Python library for interacting with the operating system.

## Environment Variables
Sensitive information such as API keys and tokens are stored in environment variables using Streamlit's secret management:

```
os.environ['OPENAI_API_KEY'] = st.secrets["openapi_key"]
zapier_client_id = st.secrets["zapier_client_id"]
zapier_redirect_uri = st.secrets["zapier_redirect_uri"]
zapier_client_secret = st.secrets["zapier_client_secret"]
azure_token = st.secrets["azure_token"]
```

## Functions
`init()`
This function sets up the Streamlit page configuration.

`on_btn_click()`
This function is called when the "Clear conversation history" button is clicked. It clears the session state messages.

`positive_feedback()`
This function is called when the user clicks the thumbs up (positive feedback) button. It sets st.session_state.messages_alr_screen to True and st.session_state.feedback_type to "Positive".

`negative_feedback()`
This function is called when the user clicks the thumbs down (negative feedback) button. It sets st.session_state.messages_alr_screen to True and st.session_state.feedback_type to "Negative".

`main()`
This function contains the core logic of the Streamlit application.

### Initialization
It initializes session state variables, such as run_main, access_token, agent_not_created, agent_chain, messages, messages_alr_screen, and feedback_type.

### Authentication with Zapier
It performs OAuth2 authentication with Zapier. The user is asked to click on a link that redirects to Zapier's authentication page. Once the user is authenticated, they're redirected to a landing page whose URL contains an authorization code. The user is asked to copy this URL and paste it into a text box in the application. The script then extracts the authorization code from the URL and exchanges it for an access token.

### AI Agent Creation
The script creates an AI agent using the access token obtained in the previous step. The agent uses Zapier to interact with Gmail and retrieve emails based on user queries. The agent also uses OpenAI's language model to generate human-like responses to the queries. The agent's creation process involves several steps, including setting up toolkits, defining prompts, initializing memory, creating language model chains, and finally initializing the agent.

### User Input Handling
The script handles user input and the AI's responses, displaying them in a chat-like interface. User input is added to the conversation history, which is stored in the session state. The AI agent processes the user input and generates a response, which is also added to the conversation history.

### Feedback Handling
The application provides thumbs up and thumbs down buttons for the user to provide feedback on the AI's responses. When a feedback button is clicked, an optional text input box appears where the user can provide additional feedback. Once the user provides feedback, it's submitted to Azure DevOps as a new work item.

### Conversation History Clearing
A "Clear conversation history" button is provided for the user to clear the conversation history. When clicked, the conversation history is cleared from the session state.

## Main Execution
Finally, the script calls the main() function if it's being run as a standalone script. This starts the Streamlit application.
