# project-alpha
This repository serves as the foundation for a new initiative focused on research, design, and creative development within the field of human rights. 
It will host documentation, data, and code that support experimental ideas, advocacy tools, and collaborative projects. 
 / import argparse

def greet(name: str) -> str:
    return f"Hello, {name}!"

def main():
    parser = argparse.ArgumentParser(description="Simple greeting CLI")
    parser.add_argument("name", help="Name to greet")
    args = parser.parse_args()
    print(greet(args.name))

if __name__ == "__main__":
    main()
import pandas as pd
from pathlib import Path

def main():
    path = Path("data/sample.csv")
    if not path.exists():
        print("Put a CSV at data/sample.csv")
        return

    df = pd.read_csv(path)
    print("Rows:", len(df))
    print("Columns:", len(df.columns))
    print("Preview:")
    print(df.head().to_string(index=False))

if __name__ == "__main__":
    main()
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
mkdir -p data
# add data/sample.csv
python src/summarize.py
document.getElementById("btn").addEventListener("click", () => {
  const title = document.getElementById("title");
  title.textContent = "Welcome to your new repo";
});
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r /dev/null  # nothing yet, keeps venv alive
python src/app.py Bilal
pytest -q  # if you install pytest
from pydantic import BaseModel, Field

class GreetRequest(BaseModel):
    name: str = Field(..., min_length=1, max_length=100)

class GreetResponse(BaseModel):
    message: str
from fastapi import FastAPI, Depends
from .models import GreetRequest, GreetResponse
from .deps import get_settings, Settings

app = FastAPI(title="Starter API", version="0.1.0")

@app.get("/health")
def health():
    return {"status": "ok"}

@app.post("/greet", response_model=GreetResponse)
def greet(payload: GreetRequest, settings: Settings = Depends(get_settings)):
    msg = f"Hello, {payload.name}! Welcome to {settings.app_name}."
    return GreetResponse(message=msg)
from functools import lru_cache
from pydantic import BaseSettings

class Settings(BaseSettings):
    app_name: str = "Rights API"
    debug: bool = False

    class Config:
        env_file = ".env"

@lru_cache
def get_settings() -> Settings:
    return Settings()
FROM python:3.11-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY app ./app
EXPOSE 8000

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
APP_NAME="Rights API"
DEBUG=true
