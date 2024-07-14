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

# Serving the LLaMa3-8b-instruct

You will need to set a Bearer token to authenticate as a [Modal Secret](https://modal.com/docs/guide/secrets).

The code currently assumes there will be a key `DSBA_LLAMA3_KEY` with a secret value (will be provided in class or you must set if you run on your own). 


```bash
modal deploy api.py
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

Alternatively, you may use [this GitHub Gist](https://gist.github.com/wesslen/16ff599634e8b1ed0ed7671ce7820f3f) after setting up the API key and `BASE_URL`.