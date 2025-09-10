1. Python 3.11+ (I used 3.13)
2. Create venv: `python -m venv .venv && source .venv/bin/activate`
3. Install deps: `pip install -r requirements.txt`
4. Set key (macOS/Linux): `echo "OPENAI_API_KEY=sk-xxx" > .env`
5. Start Jupyter: `jupyter notebook`, open `notebooks/hello_llm.ipynb`
6. Run all cells (model call, token count, cost estimate)