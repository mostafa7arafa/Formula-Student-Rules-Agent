# Formula Student Rules AI Agent

## Project Objective
The "Formula Student Rules" AI agent is designed to answer any question related to the Formula Student competition rules in Germany. It provides accurate and precise responses based on the official rulebook PDF. This tool can be particularly useful for students, competitors, and team members involved in the Formula Student competition.

## Project Architecture

The project architecture consists of one supervisor and two worker agents:

1. **Supervisor Agent:**
   - **Model**: GPT-4 mini (ChatGPT 4o mini)
   - **Temperature**: 0.5 (to balance creativity and accuracy)
   - **Role**: Oversees the flow and ensures that queries are routed to the appropriate workers. It processes the final answers provided by the workers.
   - **Prompt**:You are a supervisor tasked with managing a conversation between the following workers: {Formula Student Rules Researcher, Social Media Content Creator}.
Given the following user request, respond with the worker to act next.
Each worker will perform a task and respond with their results and status.
When finished, respond with FINISH.
Select strategically to minimize the number of steps taken.
You will start with Formula Student Rules Researcher then pass it to Social Media Content Creator

2. **Worker Agents:**
   - **Worker 1: Formula Student Rules Researcher**:
     - **Tool**: doc_ret (Retriever) 
     - **In-Memory Vector Store**: Top K = 4 results
     - **Embeddings**: Uses OpenAI's "text-embedding-ada-002"
     - **PDF Splitting**: The rulebook PDF is split recursively into chunks of 1000 tokens with a 100-token overlap, allowing the worker to search and retrieve the most relevant sections of the rules.
     - **Prompt**: As a dedicated researcher for the Formula Student team, your primary responsibility is to gather and analyze relevant information regarding the rules and regulations governing the competition. Utilizing the doc_ret tool, you will retrieve comprehensive data on the latest updates, guidelines, and requirements that teams must adhere to in order to compete successfully. Your insights will serve as the foundation for communicating critical information to the team and stakeholders.

Your goal is to compile an accurate and detailed overview of the current Formula Student rules.
Use the doc_ret tool to extract the latest information about the Formula Student competition rules, focusing on key areas such as vehicle specifications, safety requirements, and competition procedures. Ensure that the information is up-to-date and reflects any recent changes or clarifications.

Once you have gathered the necessary information, prepare a concise report summarizing the key points. This report will be essential for the next agent to create engaging content based on the retrieved data. Pass the report to the Social Media Content Creator.
   - **Worker 2: Social Media Content Creator**
     - **Prompt**: You are the creative voice of the Formula Student team, responsible for crafting engaging and informative posts that resonate with our audience. Your mission is to take the insights provided by the Formula Student Rules Researcher and transform them into captivating content that highlights the importance of adhering to competition regulations while showcasing our team's commitment to excellence.

Your goal is to create an eye-catching social media post that informs and excites our followers.
Using the summary report from the Formula Student Rules Researcher, develop a dynamic post that encapsulates the essence of the current Formula Student rules. Highlight any exciting changes or important aspects that could engage our audience and encourage them to follow our journey.

Ensure the tone is enthusiastic and aligns with our team's brand identity. Incorporate relevant hashtags and a call to action to foster engagement. Your post should not only inform but also inspire our followers to support our team as we prepare for the competition.
   
The AI agent workflow is designed to efficiently extract relevant sections from the Formula Student rulebook and return concise and accurate answers.

## Key Features
- **Custom PDF Retriever**: The rulebook is stored as chunks in memory using OpenAI embeddings, allowing for efficient retrieval of relevant content.
- **Precision**: The agent can accurately fetch rules from the document, ensuring compliance with the competition guidelines.
- **Fast Processing**: The use of in-memory vector store ensures that queries are resolved quickly by referencing only the most relevant sections.

## Agent Flow
The following diagram represents the agent flow in Flowise:

![Agent Flow](https://github.com/user-attachments/assets/7128e605-2fbc-4536-870d-8e02b5bf470a)

1. **User Query**: A user asks a question related to the Formula Student rules.
2. **Supervisor**: The supervisor agent processes the query and routes it to the researcher agent.
3. **Formula Student Rules Researcher**: The worker retrieves relevant chunks from the in-memory vector store, processes them, and returns the most relevant answers.
4. **Final Answer**: The supervisor agent finalizes the response and sends it back to the user.

## Example Outputs
Here are some example questions and the corresponding outputs provided by the AI agent:

- **Question 1**: "What are the restrictions on engine capacity in Formula Student Germany?"
  - **Answer**:
    ```
    Engine Capacity Restrictions in Formula Student Germany
    In Formula Student Germany, the following restrictions apply to engine capacity and related components:
    
    Engine Types:
    
    Vehicles can be powered by internal combustion engines (ICE) or electric powertrains.
    Restrictor Regulations:
    
    For gasoline-fueled vehicles, a maximum restrictor diameter of 20.0 mm is allowed.
    For E85-fueled vehicles, the maximum restrictor diameter is 19.0 mm.
    The restrictor must be positioned to allow for measurement during Technical Inspection and cannot be movable or flexible.
    Intake Sequence:
    
    For naturally aspirated engines, the intake sequence must be: throttle body → restrictor → engine.
    For turbocharged or supercharged engines, the sequence is: restrictor → compressor → throttle body → engine.
    Fuel Consumption:
    
    Vehicles whose corrected average fuel consumption exceeds 26 liters/100 km will receive zero points in efficiency scoring.
    These rules ensure that vehicles maintain a level of performance while adhering to safety and environmental standards. Teams must design their engines and intake systems accordingly to comply with these     specifications.
    ```
  - **Social Media Post**:
    ```
    🚗💨 Exciting Updates from Formula Student Germany! 💨🚗

    Hey, race fans! As we gear up for the thrilling competition ahead, we want to share some important engine capacity regulations that our team is diligently following to ensure we're at the top of our game! 🏁
    
    🔧 Engine Types: We can choose between internal combustion engines (ICE) or electric powertrains. The choice is ours, but the challenge is real!
    
    🌟 Restrictor Regulations:
    
    For our gasoline-powered beauties, we’re limited to a 20.0 mm restrictor diameter.
    If we opt for E85 fuel, that diameter drops to 19.0 mm. Every millimeter counts!
    📏 Intake Sequence:
    
    Naturally aspirated engines must follow: throttle body → restrictor → engine.
    Turbocharged engines need to stick to: restrictor → compressor → throttle body → engine. Precision is key!
    ⛽ Fuel Consumption:
    
    We must keep our average fuel consumption under 26 liters/100 km to score points in efficiency. Sustainability is part of our mission!
    These rules not only keep the competition fierce but also push us to innovate and excel! 💪✨
    
    Stay tuned as we navigate these regulations and showcase our journey toward excellence. Your support means the world to us! 🙌💚
    
    👉 Follow us for more updates and behind-the-scenes action! #FormulaStudent #TeamExcellence #InnovateToWin #RacingRules #SustainabilityInRacing #FuelYourPassion
    
    Let’s hit the track! 🏎️💨
    ```
- **Question 2**: "What is the minimum weight requirement for the vehicle?"
  - **Answer**:
    ```
    Minimum Weight Requirement for Formula Student Vehicle
    As per the latest Formula SAE® Rules (2022), the minimum weight requirement for the vehicle is as follows:
    
    Minimum Weight: The vehicle must weigh at least 180 kg (397 lbs) without the driver.
    This weight requirement ensures that all vehicles maintain a standard for competition, promoting fairness and safety during events. Teams must ensure that their vehicle's design and materials allow them to meet this weight specification while still adhering to other performance and safety requirements outlined in the rules.
    
    For further details on vehicle specifications and regulations, teams should refer to the full Formula SAE® rules document.
    ```
  - **Social Media Post**:
    ```
    🚗💨 Exciting News from the Formula Student Team! 💨🚗

    As we gear up for the upcoming competition, we want to keep you in the loop about the essential rules that shape our journey. One of the key regulations is the minimum weight requirement for our vehicle, which stands at 180 kg (397 lbs) without the driver! ⚖️
    
    This standard not only ensures fairness across all teams but also challenges us to innovate and optimize our designs to meet this specification while maximizing performance and safety. 💪✨
    
    Stay tuned as we dive into the details of our design process, tackle the challenges ahead, and strive for excellence on the track! Your support means the world to us, and we can’t wait to share our progress with you!
    
    Let’s get ready to race! 🏁
    
    #FormulaStudent #FSAE #Teamwork #Innovation #EngineeringExcellence #RaceReady #SupportYourTeam
    
    👉 Follow our journey and cheer us on as we push the limits of engineering!
    ```


Additional output examples can be found in the [example_outputs](./example_outputs/) folder.

## How to Run This Project
This project was created using Flowise, a no-code platform for building LangChain-based AI agents. While the project cannot be directly run from this repository, the steps below describe how to set up and replicate this project using Flowise:

### Prerequisites
1. A Flowise account (or set up Flowise locally).
2. OpenAI API key for embedding and chat models.
3. The Formula Student rules document (available in the `documents` folder).

### Setup Steps
1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/Formula-Student-Rules.git
   cd Formula-Student-Rules
