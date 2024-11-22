# Python-to-C++ Optimization Project

## Overview
This project showcases the deployment of open-source AI models to optimize Python code into high-performance C++ code. The implementation integrates advanced tools and frameworks to facilitate the transformation, benchmarking, and execution of code. It also provides a Gradio-based user interface for real-time interactions with various models.

The core functionality involves:

1. **Code Transformation**: Automatically convert Python code into highly optimized C++ code.
2. **Model Integration**: Utilize open-source models like OpenAI GPT, Anthropic Claude, and HuggingFace-hosted models.
3. **Performance Benchmarking**: Compare execution times of Python and C++ implementations.
4. **Interactive User Interface**: A Gradio-powered interface to enable seamless interaction and testing.

This README serves as a detailed guide for understanding, maintaining, and reusing the project in the future.

---

## Features

### 1. **Code Transformation**
The project uses:
- **GPT-4o (OpenAI)**
- **Claude 3.5 (Anthropic)**
- **HuggingFace-hosted models (CodeQwen and CodeGemma)**

These models take Python code as input and generate equivalent high-performance C++ code optimized for an Apple M1 processor.

### 2. **Benchmarking**
The generated C++ code is compiled using Clang++ with optimization flags:
```bash
clang++ -Ofast -std=c++17 -march=armv8.5-a -mtune=apple-m1 -mcpu=apple-m1 -o optimized optimized.cpp
```

Execution times are compared with the original Python implementation for validation and performance insights.

### 3. **User Interface**
A Gradio-based interface offers the following capabilities:
- **Convert Python to C++**: Users can input Python code and select the model to generate the C++ equivalent.
- **Execute Python Code**: Run the input Python code and view the results.
- **Execute C++ Code**: Compile and run the generated C++ code, displaying the output.

---

## Setup Instructions

### 1. **Dependencies**
Ensure you have the following installed:
- Python 3.8+
- Clang++
- Gradio
- Hugging Face Transformers
- Dotenv for environment variable management
- Required API keys (OpenAI, Anthropic, and HuggingFace)

Install the Python dependencies:
```bash
pip install -r requirements.txt
```

### 2. **Environment Variables**
Set up the `.env` file with the following keys:
```env
OPENAI_API_KEY=<your_openai_key>
ANTHROPIC_API_KEY=<your_anthropic_key>
HF_TOKEN=<your_huggingface_token>
```

### 3. **Running the Application**
Launch the Gradio interface:
```bash
python app.py
```

Access the UI at:
- Local: `http://127.0.0.1:7860`
- Public (if sharing enabled): A public link will be provided in the terminal.

---

## How It Works

### Code Workflow
1. **Input Python Code**:
   - Users provide Python code in the UI or within the predefined examples.
2. **Model Selection**:
   - Choose from GPT, Claude, or HuggingFace-hosted models for the transformation.
3. **Code Generation**:
   - The selected model generates C++ code, ensuring optimized performance for the M1 architecture.
4. **Benchmarking**:
   - Compile and run the C++ code, comparing results with the Python implementation.

### Model Integration
- **OpenAI GPT**: Uses `openai` Python library for interaction.
- **Anthropic Claude**: Uses the `anthropic` Python SDK.
- **HuggingFace Models**: Implements the `InferenceClient` for hosted inference.

### Example Tasks
#### 1. **Pi Approximation**:
Calculates Pi using a series expansion.
- Python execution time: ~48.7 seconds.
- Optimized C++ execution time: Significantly faster.

#### 2. **Maximum Subarray Sum**:
Uses a Linear Congruential Generator (LCG) to generate random numbers and calculates the maximum subarray sum.
- Python execution time: ~157.3 seconds.
- Optimized C++ execution time: Reduced drastically.

---

## Folder Structure
```plaintext
project/
├── app.py               # Main application script
├── optimized.cpp        # Generated C++ code
├── requirements.txt     # Python dependencies
├── .env                 # Environment variables
├── README.md            # Documentation
```

---

## Best Practices
1. **Endpoint Management**:
   - Ensure HuggingFace endpoints are paused when not in use to avoid unnecessary costs. Access the endpoint management UI here: [HuggingFace Endpoint Management](https://ui.endpoints.huggingface.co/).

2. **Testing**:
   - Verify correctness of generated C++ code by comparing outputs with the original Python implementation.

3. **Optimization Flags**:
   - Use appropriate compiler flags (`-Ofast`, `-march`, etc.) tailored to the target architecture.

4. **Model Selection**:
   - Choose models based on requirements: GPT for general accuracy, Claude for nuanced transformations, or HuggingFace for flexibility.

---

## Future Use Cases
- **Integration in Larger Projects**: The code generation framework can be used for other tasks requiring high-performance computation.
- **Scalability**: Extend the UI to handle batch processing or integrate with CI/CD pipelines.
- **Model Expansion**: Explore other open-source models for better efficiency and customization.

---

## Acknowledgements
- **OpenAI** for GPT APIs
- **Anthropic** for Claude APIs
- **HuggingFace** for hosting cutting-edge models
- **Gradio** for the user-friendly interface

---

## Notes for Future Reference
- Use this project as a template for similar optimization tasks.
- Revisit the README to refresh understanding of setup and workflow.
- Keep environment keys secure and endpoints paused when not in use.

For additional questions, refer to the documentation in each library or reach out to the respective support forums.

---

## Author
Cherki Meziane

