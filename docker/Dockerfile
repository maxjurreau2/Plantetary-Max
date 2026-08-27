# ---------- Stage 1: Builder ----------
FROM python:3.11-slim AS builder

WORKDIR /app

# Copy dependency file if present
COPY requirements.txt /app/requirements.txt

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt || true

# Copy the entire Portal-OS source
COPY . /app

# Optional: precompile Python bytecode
RUN python -m compileall /app


# ---------- Stage 2: Runtime ----------
FROM python:3.11-slim AS runtime

WORKDIR /app

# Copy built app from builder stage
COPY --from=builder /app /app

# Runtime environment
ENV PYTHONUNBUFFERED=1
ENV PYTHONPATH=/app

# Portal-OS HTTP handler port
EXPOSE 8000

# Entrypoint: start Portal-OS
CMD ["python", "messaging/http_handler.py"]
