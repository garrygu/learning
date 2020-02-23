

- Create Agent
- Specify Intents: actions user wishes to accomplish
- Define Entities: user parameters
- User says: Training data for NLP ML

## Agent


## Intent
- Fallback intent
- Welcome intent


## Entity
All the information we need in a user query will have a corresponding entity.
- System Entities
- Developer Entities

User entities are those that can be defined at a session level i.e. a user's playlist.


## Linear & Non-Linear Dialogs
- Contexts
  - Input Context
  - Output Context
By default, contexts expire after either five requests or ten minutes from the time they were activated.

\#context_name.parameter_name

- Follow-ups

Information relevant to the second conversation needs to flow through from the first.


## Fulfilling Intents using External APIs
Webhooks can be triggered using `POST` requests.
