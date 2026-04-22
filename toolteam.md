# AI Multi-Agent Flutter App Builder

## Overview

This project aims to build a multi-agent AI system capable of autonomously generating Flutter applications from high-level ideas. The system uses a structured pipeline of specialized agents, each responsible for a distinct part of the software development lifecycle, coordinated by a deterministic orchestrator.

The system emphasizes reliability, modularity, and grounded code generation through retrieval-augmented generation (RAG) using a local Flutter knowledge base.

---

## Goals

- Generate functional Flutter applications from user-defined ideas
- Use a structured, step-by-step execution pipeline
- Ground code generation in real Flutter documentation (PDF-based knowledge base)
- Minimize hallucinations and enforce consistency
- Provide extensibility for additional agents (debugging, testing, UI refinement)

---

## System Architecture

### Core Components

#### 1. Planner Agent
- Converts user input into a structured development plan
- Outputs granular, ordered steps with metadata (e.g., type: UI, logic, integration)

#### 2. Executor Agent
- Generates code for each step
- Uses retrieved context from the Flutter knowledge base
- Constrained to rely only on provided context

#### 3. Reviewer Agent
- Validates generated code
- Checks alignment with the plan and Flutter best practices
- Flags errors, inconsistencies, or missing elements

#### 4. Debugger Agent
- Activated only when Reviewer detects issues
- Fixes code based on feedback

#### 5. Test Generator Agent
- Produces unit/widget tests for validated code

#### 6. UI Agent
- Enhances or validates UI-related code (Flutter widgets, layouts)
- Activated only for steps labeled as UI

#### 7. Orchestrator (Non-AI)
- Controls execution flow
- Manages state and agent interactions
- Handles retries and branching logic

---

## Execution Flow

1. User provides app idea
2. Planner generates structured plan
3. For each step:
   - Retrieve relevant knowledge from vector database
   - Executor generates code
   - Reviewer validates output
   - If failed → Debugger fixes → Reviewer re-checks
   - Generate tests
   - Run UI Agent if applicable
4. Store outputs and update project state

---

## Retrieval-Augmented Generation (RAG)

### Knowledge Source
- Flutter PDF (e.g., "Fundamentals of Flutter")

### Pipeline
1. Convert PDF into text chunks
2. Embed and store in vector database
3. Retrieve relevant chunks per step
4. Provide context to Executor

### Suggested Tools
- Vector DB: FAISS or Chroma
- Framework: LlamaIndex or LangChain

---

## Orchestration Strategy

### Phase 1: Custom Python Orchestrator
- Deterministic control flow
- Explicit agent sequencing
- Easier debugging and iteration

### Phase 2: Advanced Framework (Optional)
- LangGraph for stateful workflows and branching logic

---

## Data Structures

### Plan Step Example
```json
{
  "id": 1,
  "description": "Create login screen UI",
  "type": "ui",
  "dependencies": []
}
Execution State Example
{
  "current_step": 1,
  "completed_steps": [],
  "files_generated": [],
  "errors": []
}
Constraints and Design Rules
Executor must only use retrieved context
Planner must produce granular, actionable steps
Orchestrator must remain rule-based (not AI-driven)
Agents should have narrow, well-defined responsibilities
All outputs must be validated before moving forward
Tech Stack
Language Model API: OpenAI API or local LLM
Orchestration: Python (initial), LangGraph (optional)
RAG Framework: LlamaIndex or LangChain
Vector Database: FAISS or Chroma
Target Framework: Flutter SDK
Future Enhancements
Add Debugging Agent with runtime error handling
Integrate Flutter CLI execution for real builds
Add memory system for long-term project context
Support multi-platform outputs (web, mobile, desktop)
Implement CI/CD pipeline integration
Add human-in-the-loop review interface
Development Phases
Phase 1: Foundation
Build Planner, Executor, Reviewer
Implement basic orchestrator
Set up RAG pipeline
Phase 2: Stability
Add Debugger and Test Generator
Improve validation and retry logic
Phase 3: Specialization
Add UI Agent
Improve Flutter-specific outputs
Phase 4: Scaling
Introduce LangGraph
Optimize performance and state handling
Expected Limitations
Not a full replacement for human developers
May require manual corrections for complex features
Dependent on quality of knowledge base and prompts
Flutter-specific edge cases may require iteration
