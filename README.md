# LLMPerf Leaderboard :trophy:
Utilizing the [LLMPerf](https://github.com/ray-project/llmperf), we have benchmarked a selection of LLM inference providers.
Our analysis focuses on evaluating their performance, reliability, and efficiency under the following key metrics:
- Time to first token (TTFT), which represents the duration of time that LLM returns the first token. TTFT is especially important for streaming applications, such as chatbots. 
- Inter-token latency: The average time between consecutive tokens.
- Success rate: The proportion of successful responses where the inference API operates without errors. Failures may occur due to server issues or exceeding the rate limit, reflecting the reliability and stability of API.

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

(Results as of December 22, 2023)

Note that there may be some possible source of biases or discrepancies from your perceived behavior:

1. Our measurement of TTFT depends on client location, and can also be biased by some providers lagging on the first token in order to increase ITL. Our current measurement location is from us-west (Oregon).

2. Measured ITL is not a true reflection of the system capabilities but is also impacted by the existing system load and provider traffic.


### Time to First Token (seconds)

For streaming applications, the TTFT is how long before the LLM returns the first token.

####  70B Models

<img src=".assets/ttft.jpg">


| Framework  | Model                                                                                                | Median  | Mean   | Min   | Max   | P25   | P75   | P95   | P99   |
|------------|------------------------------------------------------------------------------------------------------|---------|--------|-------|-------|-------|-------|-------|-------|
| anyscale   | meta-llama/Llama-2-70b-chat-hf                                                                      | 0.21    | 0.25   | 0.18  | 0.59  | 0.19  | 0.30  | 0.36  | 0.47  |
| bedrock    | meta.llama2-70b-chat-v1                                                                              | 0.38    | 0.40   | 0.29  | 1.16  | 0.37  | 0.41  | 0.50  | 0.60  |
| fireworks  | accounts/fireworks/models/llama-v2-70b-chat                                                          | 0.51    | 0.51   | 0.32  | 0.96  | 0.39  | 0.56  | 0.79  | 0.95  |
| lepton     | llama2-70b                                                                                           | 0.77    | 0.79   | 0.64  | 0.91  | 0.74  | 0.87  | 0.91  | 0.91  |
| perplexity | llama-2-70b-chat | 0.37 | 0.42 | 0.29 | 0.70 | 0.34 | 0.52 | 0.63 | 0.66 |
| replicate  | meta/llama-2-70b-chat                  | 13.30   | 20.12  | 1.55  | 48.91 | 9.61  | 31.29 | 47.78 | 48.83 |
| together   | together_ai/togethercomputer/llama-2-70b-chat                                                        | 0.63    | 0.62   | 0.46  | 0.89  | 0.55  | 0.67  | 0.77  | 0.87  |


#### 13B Models

<img src=".assets/ttft_13b.jpg">


| Framework  | Model                                           | Median    | Mean     | Min      | Max      | P25     | P75     | P95     | P99     |
|------------|-------------------------------------------------|-----------|----------|----------|----------|---------|---------|---------|---------|
| anyscale   | meta-llama/Llama-2-13b-chat-hf                  | 0.20      | 0.22     | 0.18     | 0.56     | 0.19    | 0.22    | 0.34    | 0.50    |
| bedrock    | meta.llama2-13b-chat-v1                         | 0.26      | 0.33     | 0.19     | 0.66     | 0.26    | 0.30    | 0.62    | 0.65    |
| fireworks  | accounts/fireworks/models/llama-v2-13b-chat     | 0.49      | 0.47     | 0.28     | 0.66     | 0.39    | 0.54    | 0.59    | 0.65    |
| lepton     | llama2-13b                                      | 0.87      | 0.87     | 0.73     | 1.04     | 0.75    | 0.94    | 1.04    | 1.04    |
| replicate  | meta/llama-2-13b-chat                           | 6.70      | 8.52     | 0.91     | 62.43    | 4.13    | 10.00   | 12.70   | 58.05   |
| together   | together_ai/togethercomputer/llama-2-13b-chat   | 0.54      | 0.89     | 0.39     | 0.91   | 0.46    | 0.60    | 0.70    | 0.81   |



> \* Perplexity doesn't offer 13B models when the data was gathered. More details for models offered could be found [here](https://docs.perplexity.ai/docs/model-cards).


#### 7B Models

<img src=".assets/ttft_7b.jpg">

| Framework | Model                                                          | Median | Mean   | Min       | Max        | P25       | P75       | P95       | P99       |
|-----------|----------------------------------------------------------------|--------|--------|-----------|------------|-----------|-----------|-----------|-----------|
| anyscale  | meta-llama/Llama-2-7b-chat-hf                                  | 0.20   | 0.23   | 0.18      | 0.50       | 0.19      | 0.23      | 0.34      | 0.46      |
| fireworks | accounts/fireworks/models/llama-v2-7b-chat                     | 0.33   | 0.33   | 0.21      | 1.09       | 0.32      | 0.34      | 0.37      | 0.88      |
| lepton    | llama2-7b                                                      | 0.85   | 0.84   | 0.63      | 1.03       | 0.73      | 0.95      | 1.00      | 1.03      |
| replicate | meta/llama-2-7b-chat | 3.50   | 3.61   | 0.90      | 11.21      | 1.58      | 4.83      | 7.40      | 9.71      |
| together  | together_ai/togethercomputer/llama-2-7b-chat                   | 0.52   | 0.58   | 0.42      | 0.95       | 0.46      | 0.71      | 0.84      | 0.94      |


> \* Perplexity doesn't offer Llama-2-7B models when the data was gathered. More details for models offered could be found [here](https://docs.perplexity.ai/docs/model-cards).

> \* Bedrock doesn't offer Llama-2-7B models when the data was gathered. More details for models offered could be found [here](https://aws.amazon.com/bedrock/llama-2/).

### Inter Token Latency (ms)

Inter-token latency is measured as the average time between consecutive tokens in a LLM request excluding the time-to-first token. We collect results by sending 150 requests to each LLM inference provider, and calculate the mean ITL and percentiles based on 150 requests.


Note that the calculation of inter-token latency (ITL) doesn't include TTFT. In the context of a response with an output token length of N, the total end-to-end response time is equal to `TTFT + (N-1) * ITL`.

####  70B Models

<img src=".assets/itl.jpg">

| Framework  | Model                                               | Median    | Mean      | Min       | Max       | P25      | P75      | P95      | P99      |
|------------|-----------------------------------------------------|-----------|-----------|-----------|-----------|----------|----------|----------|----------|
| anyscale   | meta-llama/Llama-2-70b-chat-hf                      | 14.55     | 15.84     | 11.23     | 41.82     | 13.23    | 17.06    | 22.72    | 38.74    |
| bedrock    | meta.llama2-70b-chat-v1                             | 46.53     | 47.37     | 45.03     | 52.96     | 46.03    | 48.03    | 52.02    | 52.68    |
| fireworks  | accounts/fireworks/models/llama-v2-70b-chat         | 25.17     | 25.23     | 21.98     | 30.34     | 24.00    | 26.22    | 28.02    | 29.93    |
| lepton     | llama2-70b                                          | 28.42     | 28.34     | 27.22     | 28.99     | 28.21    | 28.72    | 28.92    | 28.98    |
| perplexity | llama-2-70b-chat | 33.01 | 33.86 | 22.53 | 112.26| 32.26 | 34.86 | 38.14 | 41.17 |
| replicate  | meta/llama-2-70b-chat                               | 192.00    | 245.56    | 99.03     | 469.01    | 164.94   | 333.88   | 464.73   | 468.84   |
| together   | together_ai/togethercomputer/llama-2-70b-chat       | 15.35     | 15.80     | 12.59     | 40.12     | 14.61    | 16.20    | 19.29    | 23.01    |


####  13B Models

<img src=".assets/itl_13b.jpg">



| Framework   | Model                                              | Median     | Mean       | Min        | Max        | P25       | P75       | P95       | P99       |
|-------------|----------------------------------------------------|------------|------------|------------|------------|-----------|-----------|-----------|-----------|
| anyscale    | meta-llama/Llama-2-13b-chat-hf                     | 8.51       | 8.53       | 6.60       | 12.55      | 7.93      | 9.00      | 10.38     | 11.37     |
| bedrock     | meta.llama2-13b-chat-v1                            | 24.90      | 25.00      | 24.25      | 26.16      | 24.81     | 25.12     | 25.69     | 26.04     |
| fireworks   | accounts/fireworks/models/llama-v2-13b-chat        | 20.72      | 20.89      | 19.20      | 22.60      | 20.52     | 21.34     | 21.74     | 22.32     |
| lepton      | llama2-13b                                         | 20.56      | 20.60      | 19.58      | 22.37      | 19.81     | 20.65     | 22.34     | 22.36     |
| replicate   | meta/llama-2-13b-chat                              | 74.38      | 87.44      | 28.39      | 509.50     | 49.11     | 100.29    | 120.74    | 472.59    |
| together    | together_ai/togethercomputer/llama-2-13b-chat      | 9.56       | 18.25      | 7.89       | 22.95      | 9.02      | 10.04     | 11.96     | 19.97     |



#### 7B Models

<img src=".assets/itl_7b.jpg">

| Framework | Model                                                          | Median     | Mean       | Min        | Max        | P25       | P75       | P95       | P99       |
|-----------|----------------------------------------------------------------|------------|------------|------------|------------|-----------|-----------|-----------|-----------|
| anyscale  | meta-llama/Llama-2-7b-chat-hf                                  | 16.69      | 16.84      | 15.87      | 19.89      | 16.23     | 17.22     | 18.76     | 19.82     |
| fireworks | accounts/fireworks/models/llama-v2-7b-chat                     | 13.21      | 13.29      | 12.48      | 18.53      | 12.95     | 13.44     | 13.85     | 16.94     |
| lepton    | llama2-7b                                                      | 23.88      | 24.17      | 22.79      | 25.63      | 23.67     | 25.01     | 25.52     | 25.61     |
| replicate | meta/llama-2-7b-chat | 35.11      | 37.78      | 12.47      | 99.03      | 23.84     | 48.82     | 69.11     | 85.17     |
| together  | together_ai/togethercomputer/llama-2-7b-chat                   | 13.31      | 13.52      | 10.55      | 20.09      | 12.39     | 14.26     | 16.74     | 19.21     |



### Success Rate

The proportion of successful responses where the endpoint operates without errors. Failures may occur due to server issues or exceeding the rate limit. 

The success rate is calculated by sending 150 requests to each LLM inference provider at a fixed concurrency of 5.

####  70B Models

<img src=".assets/success_rate.jpg">

####  13B Models

<img src=".assets/success_rate_13b.jpg">

#### 7B Models

<img src=".assets/success_rate_7b.jpg">




# Feedbacks

- Kindly provide your feedback using the [link](https://github.com/ray-project/llmperf-leaderboard/issues/new). We would love to hear from you.
- For LLM inference service providers interested in having their API featured on this dashboard, please submit an issue or reach out to us via [email](mailto:endpoints-help@anyscale.com) for further communication (e.g. setting up of test accounts and etc).
