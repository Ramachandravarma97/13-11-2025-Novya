FROM python:3.10-slim

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

WORKDIR /app

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential libpq-dev curl && \
    rm -rf /var/lib/apt/lists/*

# ── IMPORTANT: context is ./backend/ai_backend ──
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy app code (no leading backend/… here)
COPY . .

EXPOSE 8001
# Flask/FastAPI: change if your app uses a different callable
CMD ["python", "-m", "uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8001"]

