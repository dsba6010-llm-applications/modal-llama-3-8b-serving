# Setup

Make sure you have signed up for a [Modal account](https://modal.com/).

To run, I'm using Python 3.10 on a Mac.

```python
python3.10 -m venv venv
source venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

To setup Modal locally:

```python
python -m modal setup
```

A browser window will open and you should select your Modal account. 

You should receive a `Web authentication finished successfully!` message.

# Serving the LLaMa3.1-8b-instruct

> [!WARNING]  
> To run this, you need to read and agree to the [LLaMa 3.1 terms](https://huggingface.co/meta-llama/Meta-Llama-3.1-8B-Instruct). Make sure your request has been approved or else you will get an error (`You must be authenticated to access it.`) deploying the app. Use Modal to register the `HF_TOKEN` to the name `llama-huggingface-secret`.

You will need to set a Bearer token (as an OPEN AI API) to authenticate as a [Modal Secret](https://modal.com/docs/guide/secrets).

You will need to name the key `DSBA_LLAMA3_KEY` with a secret value. If you're running on your own, you will need an `.env` file with `DBSA_LLAMA3_KEY=<your secret value>`. In Modal, name the key `dsba-llama3-key`. 


```bash
modal deploy src/api.py
```

This will then provide you a URL endpoint: <https://your-workspace-name--vllm-openai-compatible-serve.modal.run>

You can view Swagger <https://your-workspace-name--vllm-openai-compatible-serve.modal.run/docs>

# Running inference

Make sure you have a `.env` file with your token such that:

```
DSBA_LLAMA3_KEY=<secret-token>
```

Now, you can run:

```
$ python src/client.py
🧠: Looking up available models on server at https://your-workspace-name--vllm-openai-compatible-serve.modal.run/v1/. This may trigger a boot!
🧠: Requesting completion from model /models/NousResearch/Meta-Llama-3-8B-Instruct
👉: You are a poetic assistant, skilled in writing satirical doggerel with creative flair.
👤: Compose a limerick about baboons and racoons.
🤖: There once were two creatures quite fine,
Baboons and raccoons, a curious combine,
They raided the trash cans with glee,
In the moon's silver shine,
Together they dined, a messy entwine.
```

# Run inference in Colab

Alternatively, you may use [the repo's Jupyter notebook](/notebooks/dsba6010_openai_api_prompting_with_modal.ipynb), open in Colab, setting up the API key and `BASE_URL` via Colab, then run.

# Run inference via `curl`

```
# make sure to replace your-workspace-name
curl https://your-workspace-name--vllm-openai-compatible-serve.modal.run/v1/chat/completions \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer $DSBA_API_KEY" \
    -d '{
        "model": "/models/meta-llama/Llama-3.2-3B-Instruct",
        "messages": [
            {"role": "system", "content": "You are a helpful assistant."},
            {"role": "user", "content": "What is your training data?"}
        ]
    }' | jq
```

> [!WARNING]  
> Make sure you have a `.env` file with your token such that:
> 
> ```
> DSBA_LLAMA3_KEY=<secret-token>
> ```
