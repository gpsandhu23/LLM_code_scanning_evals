# LLM code scanning evals
Set of tests to measure effectiveness of LLMs at identifying security issues in code and generating fixes

### LLMs:
Currently setup for OpenAI GPT-4 and GPT-3.5-Turbo

### Dataset:
https://github.com/OWASP-Benchmark/BenchmarkJava

### How to run:
1. Download and open the Notebook (owasp_java_benchmark.ipynb) in your choice of Jupyter Notebook environment
2. Install the depedencies using pip install -r requirements.txt
3. Make sure the OpenAI API key is available in the execution environment as env variable OPENAI_API_KEY
4. Select the value for LLM (gpt-4-0613 or gpt-3.5-turbo-0613). GPT-4 is the newest most advanced model from OpenAI at the time of this writing, GPT3.5-Turbo is faster and cheaper
5. Set the temperature (between 0-1). This is the attribure that adds variance (highest variance at 1) to the output of the model
6. Run all the cells in the Notebook

Running all 2470 testcases on GPT-4 will cost around $100 with OpenAI API at the time of this writing. It will cost ~$5 for GPT-3.5-Turbo. More info about pricing - https://openai.com/pricing#language-models

### How to read the results:
Columns from OWASP Benchmark
1. metadata_vulnerability_exists (True/False) - This is the vulnerability tag from the XML file for the testcase that tells us if the testcase is exploitable
2. expected_vuln_type - This is the category tag from the XML file that provides the category of the vulnerability in the tesetcase

Columns from the LLM
1. vulnerability_found (True/False) - True if LLM finds a vulnerability in the code for the testcase, False otherwise
2. vulnerability - This is the category the LLM classifies the vulnerability into if it finds one for the testcase
3. vulnerable_code - This is the code sample from the testcase LLM thinks is vulnerable
4. code_fix - This is the code generated by the LLM to fix the vulnerable code
5. comment - Human readable comment that helps explain the issue and the code fix

Columns from comparison
1. vulnerability_type_matches - True if there is a 80%+ fuzzy match between expected_vuln_type (from OWASP Benchmark) and vulnerability (from the LLM)

### How are results calculated
1. True Positive - TP = ((df['vulnerability_found'] == True) & (df['metadata_vulnerability_exists'] == True)).sum()
2. True Negative - TN = ((df['vulnerability_found'] == False) & (df['metadata_vulnerability_exists'] == False)).sum()
3. False Positive - FP = ((df['vulnerability_found'] == True) & (df['metadata_vulnerability_exists'] == False)).sum()
4. False Negative - FN = ((df['vulnerability_found'] == False) & (df['metadata_vulnerability_exists'] == True)).sum()

### More information:
https://medium.com/p/9c2ca0312036

Bootsrapping this quickly. PRs welcome.
