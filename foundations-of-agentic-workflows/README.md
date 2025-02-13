<h1>
  <span class="headline">Intro to AI Agentic Workflows</span>
  <span class="subhead">Foundations of Agentic Workflows</span>
</h1>

**Learning Objective**: By the end of this lesson you'll be able to describe agentic frameworks in terms of its functional components, stages and interactions.

## An Introduction to Agentic Frameworks
Agentic frameworks are structured systems that enable AI agents to operate autonomously and collaboratively within workflows. They provide the architecture for perception, reasoning, and task execution, ensuring agents achieve specific goals efficiently. These frameworks also integrate features like memory, feedback loops, and multi-agent collaboration for adaptability and continuous learning.

---
### Core Functions of Agentic Frameworks

| Function         | Description                                                              | Example |
|-----------------|--------------------------------------------------------------------------|---------|
| | 👀 Perception | Agents gather data from their environment through APIs, user interactions, or IoT devices. | A chatbot perceives user input by interpreting natural language queries. |
| 🧠 **Reasoning**  | Agents process collected data, apply logic, and make informed decisions. | A diagnostic agent analyzes symptoms and cross-references them with a medical database to suggest potential conditions. |
| ⚡ **Action**     | Agents execute tasks or responses based on decisions made during the reasoning phase. | A logistics agent places an order for restocking inventory when stock levels are low. |
| 🔄 **Feedback Loop** | Agents refine their future behavior by learning from the outcomes of their actions. | A recommendation agent adjusts its suggestions based on customer clicks and purchases. |
| 💾 **Memory**     | Agents store and retrieve past interactions or data for improved context awareness. | A customer service agent remembers past complaints to provide better continuity in interactions. |



### **Characteristics of Agentic Frameworks**
- 🤖 **Autonomy**: Agents operate independently without requiring constant human intervention.
- 🎯 **Goal-Oriented Behavior**: Each agent is programmed to achieve specific objectives efficiently.
- 💡**Adaptability**: Agents adjust their actions dynamically based on real-time data and evolving environments.
- 🤝 **Collaboration**: Frameworks support communication between multiple agents to achieve shared goals.

---

### **Examples of Agentic Frameworks**

| Industry        | AI Agent Collaboration Example |
|---------------|----------------------------------|
| 🏥 **Healthcare**  | Diagnostic agents collaborate with patient monitoring agents to provide comprehensive care in a hospital setting. |
| 💰 **Finance**     | Fraud detection agents work alongside portfolio management agents to ensure secure and profitable financial operations. |
| 🚚 **Transportation** | Route optimization agents and fleet management agents collaborate to ensure timely and efficient deliveries. |



## Stages inside Agentic Workflows
Agentic workflows consist of interconnected stages that enable AI agents to function autonomously, collaboratively, and effectively. Each stage plays a critical role in ensuring agents can perceive, process, and act within their environment to achieve specific goals.


| **Stage** | **Description** | **Example (Smart Manufacturing System)** |
|-----------|---------------|--------------------------------------------|
| **🎯 Task Definition** | Define clear objectives and break them into actionable tasks for execution. | 🏭 *A factory AI system sets a goal to optimize production efficiency by reducing downtime and improving quality control.* |
| **📡 Data Perception & Collection** | Gather relevant data from APIs, user interactions, or IoT devices. | 📊 *Sensors collect real-time data on machine performance, product defects, and supply chain inventory.* |
| **🧠 Processing & Reasoning** | Analyze collected data using rule-based logic, machine learning, or statistical methods. | 🔍 *Anomaly detection models analyze sensor data to predict potential machine failures.* |
| **🤖 Decision-Making** | Make informed decisions aligned with workflow objectives based on data insights. | ⚙️ *The AI system decides whether to schedule preventive maintenance or adjust production settings to optimize efficiency.* |
| **⚡ Task Execution** | Perform specific actions like sending alerts, updating systems, or interacting with users. | 🚨 *A maintenance agent automatically schedules a repair task and alerts technicians before a breakdown occurs.* |
| **🔗 Collaboration Between Agents** | Enable multiple agents to share information and coordinate tasks. | 🤝 *The maintenance agent communicates with the supply chain agent to ensure spare parts are ordered on time.* |
| **🔄 Feedback Loop** | Continuously refine models and improve performance using outcome-based feedback. | 🔄 *The AI system analyzes repair effectiveness and updates predictive models to improve future maintenance scheduling.* |
| **📈 Monitoring & Adaptation** | Track workflow performance in real-time and adapt to new data or conditions. | 📡 *The AI system dynamically adjusts production schedules based on real-time machine performance and demand forecasts.* |



### **Benefits of These Stages**
- **Efficiency**: Each component works together to streamline operations and reduce redundancy.  
- **Scalability**: Enables workflows to handle increasing complexity or demand without significant redesign.  
- **Resilience**: Agents can adapt to unforeseen changes, ensuring workflow continuity.


## **Modes of Interaction:** How AI Agents Interact with Systems and Users
AI agents act as **intermediaries between users and systems**, processing inputs, making decisions, and delivering outputs to achieve specific goals. Their interaction mechanisms are designed to ensure seamless communication, efficiency, and adaptability.


| **Interaction Type**       | **Description** | **Key Features** | **Example** |
|---------------------------|----------------|------------------|-------------|
| **🗣️ Interaction with Users** | AI agents communicate with users via natural language, graphical interfaces, or voice-based systems. | ✔️ Interprets user queries.  <br> ✔️ Tailors responses based on user behavior.  <br> ✔️ Anticipates needs and offers solutions. | ✔️ 🏪 *A customer service chatbot resolves user complaints by accessing relevant information.*  <br> ✔️ 📅 *A virtual assistant schedules appointments proactively.* |
| **🔗 Interaction with Systems** | Agents interface with databases, APIs, IoT devices, and enterprise systems to collect data, process tasks, and execute actions. | ✔️ Connects with multiple data sources.  <br> ✔️ Uses REST or GraphQL APIs for data retrieval.  <br> ✔️ Responds to live data streams. | - 🚚 *A logistics agent monitors deliveries through a fleet management system.*  <br> - 🌦️ *A weather forecasting agent updates predictions using IoT sensor data.* |
| **🤝 Multi-Agent Collaboration** | Multiple agents work together to handle complex, interdependent tasks efficiently. | ✔️ Exchanges data for coordinated actions.  <br> ✔️  Assigns tasks to specialized agents. | ✔️ 🏭 *In a smart factory, a production scheduling agent collaborates with a maintenance agent to prevent downtime.* |


### **Challenges in Interactions**
**1. Context Understanding**:  
- Ensuring agents accurately interpret ambiguous or incomplete inputs.  
- Example: A chatbot may misinterpret a vague query like "It’s not working" without additional context.

**2. Data Integration**:  
- Connecting to disparate systems with varying data formats and protocols.  
- Example: A financial agent may struggle to retrieve data from outdated legacy systems.

**3. Security and Privacy**:  
- Safeguarding sensitive information during interactions.  
- Example: Ensuring encrypted communication between agents and financial databases.

### **Benefits of Effective Interaction**

- **Seamless User Experience**: Enhances satisfaction by providing quick and accurate responses.  
- **Operational Efficiency**: Automates data collection and decision-making across systems.  
- **Scalability**: Handles large volumes of user queries or system requests effortlessly.  


## 🗣️ **Discussion Activity**: ShopSmart
 ShopSmart's agentic workflow is designed to facilitate customer support and order tracking. The agents are connected to the company's inventory management system and CRM. When a customer asks about product availability, the agent accesses real-time data to provide an accurate response.

 🤔 **Think About It** What, if any, should the proccess be to handle situations where a customer support agent isn't able to handle a customers needs?
