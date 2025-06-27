### **Reproducibility**

This document outlines the steps to reproduce the extraction of a VA Blueprint from a given research paper using a Large Language Model (LLM).

#### **1. Required Materials**

You will need three files for each extraction task. All needed files (but the academic papers) are available in the current repository:
*   **`schema.json`**: This file defines the strict JSON schema that the final output must follow.
*   **`few_shot_samples.json`**: This file contains a set of previously extracted system blueprints.
*   **A target research paper (PDF)**: The academic paper from which the VA Blueprint will be extracted.

#### **2. Model Requirements**

*   **File Upload Capability**: The LLM interface you use must support the uploading of multiple files, specifically PDF and JSON formats, within a single session.
*   **Model Choice**: This process was developed and validated using OpenAI's **GPT-4** model. We also successfully performed some tests using the methodology with other models (older GPT versions, Claude, Gemini). We encourage you to test more recent models with larger context windows such as Gemini 2.5 Pro.

#### **3. Step-by-Step Execution**

To perform an extraction, follow these steps precisely:
1.  Start a new, clean chat session with your chosen LLM.
2.  Upload the `schema.json` file.
3.  Upload the `few_shot_samples.json` file.
4.  Upload the target research paper PDF you wish to analyze.
5.  Copy the entire prompt below and paste it into the chat.

---

### **LLM PROMPT**

You are an expert research assistant specializing in the systematic analysis of Visual Analytics (VA) systems. Your task is to deconstruct a research paper describing a VA system and represent its architecture as a structured JSON object (VA Blueprint).

Your response MUST be a single, valid JSON object that strictly adheres to the provided `schema.json`.

You have access to two essential files:
1.  **`schema.json`**: The strict structural template for your output. Pay close attention to the descriptions of each field.
2.  **`few_shot_samples.json`**: A knowledge base of previously extracted systems. This is your primary source for few-shot examples to ensure consistency.

**Your Step-by-Step Process:**

**Step 1: Comprehend the System Architecture**
*   Read the provided PDF thoroughly. Focus on sections detailing the system's methodology, architecture, implementation, components, and user interface.
*   Identify all the distinct components of the system. Think in terms of data inputs, processing steps, analytical methods, visualization views, and user interactions.

**Step 2: Classify Components into the Hierarchy**
*   Organize the components you identified into the three-level hierarchy defined in `schema.json`:
    *   **HighBlocks**: The top-level categories. These are almost always `Data Loading`, `Data Processing`, `Visualization`, and `Interaction`.
    *   **IntermediateBlocks**: Functional groupings within a HighBlock. **Consult `few_shot_samples.json`** to see common examples (e.g., under `Data Processing`, you might find `Clustering`, `Feature Extraction`, `Indexing`). Use these names when applicable for consistency.
    *   **GranularBlocks**: The most specific components.

**Step 3: Detail Each Granular Block (IMPORTANT INSTRUCTIONS)**
*   For each `GranularBlock`, you must meticulously fill out the following fields from the schema:
    *   **`GranularBlockName`**: Give a concise, descriptive name. **Look at `few_shot_samples.json` for naming conventions**. For example, instead of "Map showing things", use "Map 2D". Instead of "Graph of nodes", use "Graph" or "Node-Link Diagram".
    *   **`ID`**: Assign a unique integer ID to each Granular Block, starting from 1 for each new paper.
    *   **`PaperDescription`**: Provide a 1-2 sentence summary of what the block does, based *only* on the information in the paper.
    *   **`Inputs` & `Outputs`**: List the specific data or signals that this block consumes and produces. Be precise (e.g., Input: "Raw taxi trip records", Output: "Clustered traffic regions").
    *   **`ReferenceCitation`**: **This is the most important field for reproducibility.** Find the sentence or two in the paper that *best proves the existence and function* of this block.
        *   **It MUST be a direct, verbatim quote.** Do not change a single word.
        *   **It MUST include the section or figure number** where the quote was found. (e.g., "Section 4.2: 'The system uses a k-d tree to index the spatial data.'").
    *   **`FeedsInto`**: This defines the dataflow. If the output of Block `ID: 5` is used as an input for Block `ID: 8`, then the `FeedsInto` array for Block 5 should contain `[8]`. Trace the entire system pipeline to complete this accurately.

**Step 4: Final JSON Construction & Validation**
*   Assemble all the extracted information into a single JSON object.
*   Before providing the final output, mentally validate it against the `schema.json` to ensure all required fields are present and correctly typed. Ensure all `FeedsInto` IDs are valid.

Begin the extraction for the provided paper now.