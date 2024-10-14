# Assignment Overview

As part of the co-pilot agent, which gets a user query in natural language and translates it into the system’s API request (in JSON format), one of the most important tasks is to correctly identify the relevant entities.

Your task is to build a pipeline that, given an input user query, predicts the relevant entities. The goal is to **predict** which entities are mentioned or implied by the user’s query.

## Dataset Description

Two CSV files are provided to assist you in this task:

1. **user_queries.csv**:
    - **Columns**:
        - `questions`: Contains the user’s textual query (e.g., "Show me all reports related to malware infection").
        - `json`: Contains the corresponding JSON object that represents the API query matching the user question. Inside the JSON object, the relevant entities can be found in the `entityType` key and, if present, also in the `relationTargetType` key (which are part of the JSON object).

2. **fields_description.csv**:
    - **Purpose**: This file provides details for each type of entity, including the relevant fields associated with the entity and a textual description of each field. While this information can help enhance the accuracy of entity prediction, it is not mandatory to use it.

## Objective

The primary objective is to create an optimal pipeline that can predict the relevant entities based on the input query using a **Large Language Model (LLM)**. You should:

1. **Use a Small LLM**: Preferably select a small model that can fit on a CPU, or a GPU if available.
2. **Run the Entire Pipeline Locally**: Ensure that all components of the pipeline are executed on a local machine.

### Key Notes:
- The entities can be found in the `entityType` key and, if present, also in the `relationTargetType` key of the JSON column.
- A pipeline that relies solely on prompt engineering will not be sufficient for this assignment.

---

## Example of Expected Input, JSON, and Expected Output

<table>
  <tr>
    <th>Use query (input)</th>
    <th>JSON</th>
    <th>Expected output</th>
  </tr>
  <tr>
    <td>What SMS messages were sent from suspicious phones to 0549876543 containing the word 'urgent'?</td>
    <td>
      <pre><code class="language-json">
{
  "entityType": "CDR",
  "statements": [
    {
      "type": "filter",
      "parameters": {
        "name": "ifc.ootb.CDR.msisdn2",
        "operator": "equals",
        "value": "0549876543"
      }
    },
    {
      "type": "filter",
      "parameters": {
        "name": "ifc.ootb.CDR.smsText",
        "operator": "contains",
        "value": "urgent"
      }
    },
    {
      "type": "filter",
      "parameters": {
        "name": "ifc.ootb.CDR.type",
        "operator": "equals",
        "value": "Text"
      }
    },
    {
      "type": "relation",
      "parameters": {
        "relationType": ["relation_Caller"],
        "relationTargetType": ["Phone"]
      },
      "statements": [
        {
          "type": "filter",
          "parameters": {
            "name": "ifc.ootb.Participant."
          }
        }
      ]
    }
  ]
}
      </code></pre>
    </td>
    <td><code>['CDR', 'Phone']</code></td>
  </tr>
</table>

---

## Deliverables

1. **Python Script or Jupyter Notebook**: Provide your implementation.
2. **Explanation of Approach**: Include a brief explanation of the techniques applied and the justifications for your choices, including the metrics used to evaluate performance.
3. **Test Cases**: Run your pipeline on several test queries and provide the predicted entities.
4. **Open Issues and Suggestions for Improvement**: Highlight any open issues and provide further suggestions for future improvements.
