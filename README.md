# Zeca Bot: Conversational Finance Assistant (N8N + Ollama)

A simple Proof of Concept (POC) using the N8N AI Agent, a local Ollama LLM, and N8N Data Tables to manage monthly bills via a chat interface.

## 🚀 About This Project

This project is a personal finance assistant named "Zeca." It uses the N8N `Chat Trigger` node for a simple UI, an `AI Agent` to understand natural language, and a local LLM (like `qwen3:4b`) running on Ollama to power the agent.

All bill information (name, amount, due date, paid status) is stored and managed (CRUD) using N8N's built-in **Data Tables** feature.

## 🛠️ Tech Stack

* **Automation:** [N8N](https://n8n.io/)
    * `Chat Trigger`
    * `AI Agent` node
    * `Ollama Chat Model` node
    * `Data Table Tool` node
* **LLM Serving:** [Ollama](https://ollama.com/)
* **Model:** `qwen3:4b` (or any other function-calling model)
* **Database:** N8N [Data Tables](https://docs.n8n.io/data/data-tables/)

## ⚙️ How It Works

1.  A user sends a message (e.g., "add my power bill, $150, due tomorrow") via the **Chat Trigger**.
2.  The **AI Agent** node receives the message.
3.  The agent, powered by the **Ollama Chat Model**, analyzes the text and the current date (passed in the system prompt).
4.  It intelligently determines the user's intent and extracts the data (e.g., `name: "power bill"`, `amount: 150`).
5.  It automatically calls the **Data Table Tool**, which is configured to manage the `monthly_bills` table.
6.  The tool performs the correct database operation (Insert, Get, Update, or Delete).
7.  The agent sends the confirmation message (e.g., "Successfully inserted 1 row") back to the chat.

## 🔧 Setup

1.  **Ollama:**
    * Install and run Ollama on your local machine.
    * Pull the model: `ollama pull qwen3:4b`

2.  **N8N:**
    * Run N8N (this POC assumes you are using Docker).
    * On the left-hand menu, go to **Data Tables** and create a new table named `monthly_bills`.
    * Add the following columns:
        * `name` (String)
        * `amount` (Number)
        * `due_date` (String)
        * `is_paid` (Boolean, default `false`)

3.  **Workflow Configuration:**
    * Import the `work_flows/CRUD_Telegram.json`.
    * In the **Ollama Chat Model** node, set the **Base URL** to `http://host.docker.internal:11434`. This allows the N8N Docker container to access Ollama running on your host machine.
    * In the **AI Agent** node, connect the **Data Table Tool** node to its `Tool` input.
    * Configure the **Data Table Tool** to use your `monthly_bills` table and add the custom descriptions for each field.

## 💬 How to Use

Once the workflow is active, open the `Chat Trigger` chat panel and start sending commands in (Brazilian) Portuguese:

* `"Adicionar minha conta de luz, R$150, vencimento dia 10"`
* `"Quais contas eu tenho que pagar?"`
* `"Eu paguei a Netflix"`
* `"Excluir a conta de aluguel"`

Criado com 🧠 por **Alex Bispo**.
