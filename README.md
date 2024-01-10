# LLMPerf Leaderboard :trophy:
Utilizing the [LLMPerf](https://github.com/ray-project/llmperf), we have benchmarked a selection of LLM inference providers.
Our analysis focuses on evaluating their performance, reliability, and efficiency under the following key metrics:
- Output tokens throughput, which represents the average number of output tokens returned per second. This metric is important for applications that require high throughput, such as summarization and translation, and easy to compare across different models and providers. 
- Time to first token (TTFT), which represents the duration of time that LLM returns the first token. TTFT is especially important for streaming applications, such as chatbots. 

The LLMPerf Leaderboard displays results in a clear, transparent manner. Our aim is to provide users and developers with vital insights into the capabilities and limitations of each provider, informing decisions for future integrations and deployments. In line with our commitment to transparency and utility, we also provide reproducible steps in [Run Configurations](#run-configurations) as shown below:

#### Run Configurations

For each of the benchmark run, it is performed with the below command template from the [LLMPerf repository](https://github.com/ray-project/llmperf)

```
   python token_benchmark_ray.py \
    --model <MODEL_NAME> \
    --mean-input-tokens 550 \
    --stddev-input-tokens 0 \
    --mean-output-tokens 150 \
    --stddev-output-tokens 0 \
    --max-num-completed-requests 150 \
    --num-concurrent-requests 5 \
    --llm-api <litellm/openai> 
```

For each provider, we perform:
- Total number of requests:     150
- Concurrency:                  5 (5 concurrent requests to the provider)
- Prompt's token length:        550
- Expected output length:       150
- Tested models:                7B, 13B, and 70B of LLama-2 chat models


We ran the LLMPerf clients on an AWS EC2 (Instance type: i4i.large) from us-west-2 (Oregon) region. The results were up-to-date of December 19, 2023, 3am PST. You could find the detailed results in the [raw_data](raw_data) folder.

#### Caveats and Disclaimers

Note that there may be some possible source of biases or discrepancies from your perceived behavior:

- The endpoints provider backend might vary widely, so this is not a reflection on how the software runs on a particular hardware.
- The results may vary with time of day.
- The results (e.g. measurement of TTFT) depend on client location, and can also be biased by some providers lagging on the first token in order to increase ITL. Our current measurement location is from us-west (Oregon).
- The results is only a proxy of the system capabilities and is also impacted by the existing system load and provider traffic.
- The results may not correlate with users’ workloads.


### Output Tokens Throughput (tokens/s)

The output tokens throughput is measured as the average number of **output** tokens returned per second. We collect results by sending 150 requests to each LLM inference provider, and calculate the mean output tokens throughput based on 150 requests. A higher output tokens throughput indicates a higher throughput of the LLM inference provider. 

####  70B Models

<img src=".assets/output_tokens_per_s.jpg">

| Framework   | Model                                         |   Median |   Mean |   Min |   Max |   P25 |   P75 |   P95 |   P99 |
|:------------|:----------------------------------------------|---------:|-------:|------:|------:|------:|------:|------:|------:|
| anyscale    | meta-llama/Llama-2-70b-chat-hf                |       66 |     63 |    22 |    86 |    56 |    72 |    77 |    82 |
| bedrock     | meta.llama2-70b-chat-v1                       |       21 |     21 |    13 |    22 |    20 |    22 |    22 |    22 |
| fireworks   | accounts/fireworks/models/llama-v2-70b-chat   |       40 |     40 |    33 |    46 |    38 |    42 |    45 |    46 |
| lepton      | llama2-70b                                    |       33 |     33 |    31 |    39 |    32 |    34 |    34 |    38 |
| perplexity  | llama-2-70b-chat                              |       30 |     30 |     8 |    44 |    29 |    31 |    36 |    44 |
| replicate   | meta/llama-2-70b-chat                         |       10 |      9 |     2 |    11 |    10 |    10 |    11 |    11 |
| together    | together_ai/togethercomputer/llama-2-70b-chat |       65 |     64 |    25 |    79 |    61 |    68 |    74 |    76 |
| groq        | llama2-70b-4096                               |      185 |    184 |   148 |   208 |   174 |   195 |   207 |   208 |

####  13B Models

<img src=".assets/output_tokens_per_s_13b.jpg">

| Framework   | Model                                         |   Median |   Mean |   Min |   Max |   P25 |   P75 |   P95 |   P99 |
|:------------|:----------------------------------------------|---------:|-------:|------:|------:|------:|------:|------:|------:|
| anyscale    | meta-llama/Llama-2-13b-chat-hf                |      120 |    120 |    81 |   156 |   110 |   128 |   141 |   148 |
| bedrock     | meta.llama2-13b-chat-v1                       |       36 |     35 |    19 |    39 |    33 |    38 |    38 |    39 |
| fireworks   | accounts/fireworks/models/llama-v2-13b-chat   |       42 |     42 |    39 |    45 |    41 |    43 |    44 |    44 |
| lepton      | llama2-13b                                    |       43 |     43 |    37 |    48 |    42 |    44 |    46 |    48 |
| replicate   | meta/llama-2-13b-chat                         |       16 |     18 |     6 |    35 |    12 |    20 |    35 |    35 |
| together    | together_ai/togethercomputer/llama-2-13b-chat |      102 |    101 |     1 |   123 |    98 |   108 |   119 |   122 |

#### 7B Models

<img src=".assets/output_tokens_per_s_7b.jpg">

| Framework   | Model                                        |   Median |   Mean |   Min |   Max |   P25 |   P75 |   P95 |   P99 |
|:------------|:---------------------------------------------|---------:|-------:|------:|------:|------:|------:|------:|------:|
| anyscale    | meta-llama/Llama-2-7b-chat-hf                |       51 |     51 |    45 |    57 |    49 |    54 |    56 |    57 |
| fireworks   | accounts/fireworks/models/llama-v2-7b-chat   |       76 |     76 |    53 |    82 |    75 |    78 |    79 |    82 |
| lepton      | llama2-7b                                    |       36 |     36 |    33 |    40 |    35 |    38 |    40 |    40 |
| replicate   | meta/llama-2-7b-chat                         |       26 |     32 |     2 |    78 |    20 |    35 |    73 |    77 |
| together    | together_ai/togethercomputer/llama-2-7b-chat |       75 |     75 |    50 |    95 |    70 |    81 |    87 |    90 |

### Time to First Token (seconds)

For streaming applications, the TTFT is how long before the LLM returns the first token.

####  70B Models

<img src=".assets/ttft.jpg">


| Framework  | Model                                                                                                | Median  | Mean   | Min   | Max   | P25   | P75   | P95   | P99   |
|------------|------------------------------------------------------------------------------------------------------|---------|--------|-------|-------|-------|-------|-------|-------|
| anyscale   | meta-llama/Llama-2-70b-chat-hf                                                                      | 0.21    | 0.25   | 0.18  | 0.59  | 0.19  | 0.30  | 0.36  | 0.47  |
| bedrock     | meta.llama2-70b-chat-v1 |     0.39 |   0.41 |  0.29 |  0.72 |  0.37 |  0.41 |  0.54 |  0.69 |
| fireworks  | accounts/fireworks/models/llama-v2-70b-chat                                                          | 0.51    | 0.51   | 0.32  | 0.96  | 0.39  | 0.56  | 0.79  | 0.95  |
| lepton      | llama2-70b |     0.93 |    0.9 |  0.72 |  1.12 |  0.82 |  0.96 |  1.01 |   1.1 |
| perplexity | llama-2-70b-chat | 0.37 | 0.42 | 0.29 | 0.70 | 0.34 | 0.52 | 0.63 | 0.66 |
| replicate   | meta/llama-2-70b-chat |     1.19 |   5.08 |  0.97 | 71.57 |  1.03 |   1.7 | 24.23 | 63.63 |
| together   | together_ai/togethercomputer/llama-2-70b-chat                                                        | 0.63    | 0.62   | 0.46  | 0.89  | 0.55  | 0.67  | 0.77  | 0.87  |
i| groq        | llama2-70b-4096                               |     0.22 |   0.23 |  0.17 |  0.36 |  0.19 |  0.24 |  0.3  |  0.35 |

#### 13B Models

<img src=".assets/ttft_13b.jpg">


| Framework  | Model                                           | Median    | Mean     | Min      | Max      | P25     | P75     | P95     | P99     |
|------------|-------------------------------------------------|-----------|----------|----------|----------|---------|---------|---------|---------|
| anyscale   | meta-llama/Llama-2-13b-chat-hf                  | 0.20      | 0.22     | 0.18     | 0.56     | 0.19    | 0.22    | 0.34    | 0.50    |
| bedrock     | meta.llama2-13b-chat-v1 |     0.27 |   0.33 |  0.16 |  0.77 |  0.25 |   0.3 |  0.74 |  0.76 |
| fireworks  | accounts/fireworks/models/llama-v2-13b-chat     | 0.49      | 0.47     | 0.28     | 0.66     | 0.39    | 0.54    | 0.59    | 0.65    |
| lepton      | llama2-13b |     1.08 |   1.07 |  0.82 |   1.4 |  0.95 |  1.15 |  1.24 |  1.37 |
| replicate   | meta/llama-2-13b-chat |     5.65 |   6.27 |  0.98 | 17.01 |  3.62 |  8.31 | 14.76 | 16.71 |
| together   | together_ai/togethercomputer/llama-2-13b-chat   | 0.54      | 0.89     | 0.39     | 0.91   | 0.46    | 0.60    | 0.70    | 0.81   |



> \* Perplexity doesn't offer 13B models when the data was gathered. More details for models offered could be found [here](https://docs.perplexity.ai/docs/model-cards).


#### 7B Models

<img src=".assets/ttft_7b.jpg">

| Framework | Model                                                          | Median | Mean   | Min       | Max        | P25       | P75       | P95       | P99       |
|-----------|----------------------------------------------------------------|--------|--------|-----------|------------|-----------|-----------|-----------|-----------|
| anyscale  | meta-llama/Llama-2-7b-chat-hf                                  | 0.20   | 0.23   | 0.18      | 0.50       | 0.19      | 0.23      | 0.34      | 0.46      |
| fireworks | accounts/fireworks/models/llama-v2-7b-chat                     | 0.33   | 0.33   | 0.21      | 1.09       | 0.32      | 0.34      | 0.37      | 0.88      |
| lepton      | llama2-7b |     1.13 |   1.11 |  0.88 |  1.33 |  1.04 |  1.18 |  1.29 |  1.32 |
| replicate   | meta/llama-2-7b-chat |     3.68 |   3.61 |  0.99 |   7.2 |  2.31 |  5.01 |  6.37 |  6.99 |
| together  | together_ai/togethercomputer/llama-2-7b-chat                   | 0.52   | 0.58   | 0.42      | 0.95       | 0.46      | 0.71      | 0.84      | 0.94      |


> \* Perplexity doesn't offer Llama-2-7B models when the data was gathered. More details for models offered could be found [here](https://docs.perplexity.ai/docs/model-cards).

> \* Bedrock doesn't offer Llama-2-7B models when the data was gathered. More details for models offered could be found [here](https://aws.amazon.com/bedrock/llama-2/).


# Feedback

- Kindly provide your feedback using the [link](https://github.com/ray-project/llmperf-leaderboard/issues/new). We would love to hear from you.
 - For LLM inference service providers interested in having their API featured on this dashboard, please submit an issue or reach out to us via [email](mailto:endpoints-help@anyscale.com) for further communication (e.g. setting up of test accounts and etc).
