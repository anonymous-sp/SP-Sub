# Prompt Implementation for the Estimation of User Motivation and Capability

To dynamically assess a user's motivation and capability within the conversation, we utilized a state-of-the-art instruction-tuned LLM with robust natural language understanding and reasoning capabilities. The LLM processed the ongoing conversation history ($h$) to generate normalized scores (ranging from 0.0 to 1.0) for both dimensions. This methodology leverages the LLM's capacity to infer nuanced user states from dialogue patterns without requiring explicit user input or pre-defined linguistic feature engineering. The specific procedures for each dimension are detailed below:

## Estimating User Motivation

- Definition: Motivation was defined as the user's willingness and eagerness to disclose privacy-related information.

- Input to LLM: The full conversational history ($h$) up to the user's latest turn.

- Prompting strategy: The LLM was prompted with instructions to:

1. Analyze the user's contributions throughout the conversation history. 

2. Identify explicit and implicit cues indicating their willingness to share personal details. Key aspects considered included voluntariness of disclosure, which is instances where the user provided personal information beyond the direct requirements of the chatbot's queries; enthusiasm and openness, which is the sentiment expressed when discussing personal topics, positive or neutral sentiments were considered indicative of higher motivation compared to hesitant or reluctant expressions; elaboration and detail, which is the depth and richness of details provided in self-disclosures; proactiveness in sharing, which is whether the user initiated the sharing of personal information or actively steered the conversation towards personal subjects; response latency, while not directly measurable from text alone in this setup, it allows LLMs to infer response patterns that might correlate with eagerness. 

3. Based on this holistic analysis, LLMs output a numerical score between 0.0 (indicating very low eagerness or unwillingness to share) and 1.0 (indicating very high eagerness and willingness to share).

- Output: A single floating-point score presenting the estimated motivation level.

## Estimating User Capability (Precision of Privacy Information)}

- Definition: Capability was defined as the effectiveness and precision of the users' disclosure of private information, reflecting how well they articulate such information.

- Input to LLM: The full conversation history ($h$). 

- Prompting strategy: The LLM was prompted with instructions to:

1. Analyze the content and structure of the privacy-related information disclosed by the user. 

2. Evaluate the quality and potential utility of this information based on its precision. Key aspects considered included specificity and unambiguity, which is the degree to which the disclosed information was precise (e.g., "my email is name\@domain.com" vs. "I use email"; "born on 15th May 1990" vs. "in the 90s"); completeness and actionability, which is whether the shared information was sufficiently complete to be considered useful or actionable in a real-world context (e.g., a full address vs. only a city name); clarity of articulation, which is the lucidity with which the information was presented, free from vagueness or deliberate obfuscation; effective leverage in context, which is how well the user utilized their private information to answer questions or contribute to the conversation, even if the disclosure was unintentional but clear and precise. 

3. Based on this analysis, output a numerical score between 0.0 (indicating very low precision of ineffective disclosure) and 1.0 (indicating very high precision and effective disclosure). 

- Output: A single floating-point score representing the estimated capability level.

## LLM Configuration and Output Interpretation

For these estimation tasks, an LLM was employed in a zero-shot inference mode. The prompt were carefully designed to directly elicit the normalized scores (0.0-1.0) for motivation and capability. These scores were then consumed by the "SelectStrategy" algorithm, where the empirically determined threshold of 0.7 was used to classify users into high/low states for each dimension, thereby guiding the strategy selection. No model fine-tuning was preformed for these specific state estimation functions, relying instead on the LLM's inherent few-shot or zero-shot reasoning abilities developed during its extensive pre-training.


